<template>
    <div class="all-elevators-container">
        <div v-for="elevator in numberOfElevators" :key="elevator">
            <SingleElevator
                ref="elevators"
                :id="elevator"
                :numberOfLevels="numberOfLevels"
            />
        </div>
    </div>
</template>

<script>
import SingleElevator from "./SingleElevator.vue";

export default {
    name: "AllElevators",
    data() {
        return {
            executingCalls: [],
            pendingCalls: [],
        };
    },
    watch: {
        executingCalls: {
            deep: true,

            handler() {
                localStorage.setItem(
                    "executingCalls",
                    JSON.stringify(this.executingCalls)
                );
            },
        },

        pendingCalls: {
            deep: true,

            handler() {
                localStorage.setItem(
                    "pendingCalls",
                    JSON.stringify(this.pendingCalls)
                );
            },
        },
    },
    components: {
        SingleElevator,
    },
    props: ["numberOfElevators", "numberOfLevels", "executionQueue"],

    methods: {
        getElevator(level) {
            let nearestElevator = this.getNearestElevator(level);
            if (
                nearestElevator &&
                !nearestElevator.elevatorIsBusy()
            ) {
                return nearestElevator;
            } else return null;
        },

        getNearestElevator(level) {
            let distances = [];

            for (let i in this.$refs.elevators) {
                if (!this.$refs.elevators[i].elevatorIsBusy()) {
                    let elevatorPosition =
                        this.$refs.elevators[i].getPosition();
                    let distance = Math.abs(level - elevatorPosition);
                    distances.push({ distance: distance, elevator: this.$refs.elevators[i] });
                }
            }
            if (distances.length > 0) {
                distances.sort((a, b) => a.distance - b.distance);
                return distances[0].elevator;
            }
            else return null;
        },

        addToQueue(call) {
            this.pendingCalls.push(call);
        },

        areElevatorsAtLevel(level) {
            for (let i in this.$refs.elevators) {
                if (
                    level === this.$refs.elevators[i].getPosition() &&
                    !this.$refs.elevators[i].elevatorIsBusy()
                ) {
                    return true;
                }
            }
            return false;
        },

        handleCall(call) {
            let elevator = this.getElevator(call);
            if (elevator && !this.areElevatorsAtLevel(call)) {

                if(!this.executingCalls.includes(call)) {
                    this.executingCalls.push(call);
                }
                if (this.pendingCalls.length > 0) {
                    this.pendingCalls.shift();
                }
                elevator.initNewCall(call);
            } else if (elevator && this.areElevatorsAtLevel(call)) {
                return this.$parent.removeFromMainQueue(call);
            }
            else if (!elevator) {
                this.addToQueue(call);
            }
        },

        removeFromExecuting(finishedCall) {
            if (this.executingCalls.length === 1) {
                this.executingCalls.shift();
            } else {
                this.executingCalls = this.executingCalls.filter((call) => {
                    return call !== finishedCall;
                });
            }
        },

        nextCall(currentCall) {
            this.$parent.removeFromMainQueue(currentCall);
            this.removeFromExecuting(currentCall);

            if (this.pendingCalls.length > 0) {
                this.handleCall(this.pendingCalls[0]);
            } else {
                return;
            }
        },
    },

    mounted() {
        if (localStorage.pendingCalls) {
            this.pendingCalls = JSON.parse(
                localStorage.getItem("pendingCalls")
            );
        }
        if (localStorage.executingCalls) {
            

            this.executingCalls = JSON.parse(
                localStorage.getItem("executingCalls")
            );
        }
    }

};
</script>

<style scoped>
.all-elevators-container {
    display: flex;
}
</style>
