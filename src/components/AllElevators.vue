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
            executingElevators: []
            
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

        executingElevators: {
            deep: true,

            handler() {
                localStorage.setItem(
                    "executingElevators",
                    JSON.stringify(this.executingElevators)
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

        getElevatorIndex(level) {
            let nearestElevatorIndex = this.getNearestElevator(level);
            if (
                nearestElevatorIndex &&
                !this.$refs.elevators[nearestElevatorIndex].elevatorIsBusy() 
            ) {
                return nearestElevatorIndex;
            } else return null;
        },

        getNearestElevator(level) {
            let distances = [];

            for (let i in this.$refs.elevators) {
                if (!this.$refs.elevators[i].elevatorIsBusy()) {
                    let elevatorPosition =
                        this.$refs.elevators[i].getPosition();
                    let distance = Math.abs(level - elevatorPosition);
                    distances.push({ distance: distance, elevatorIndex: i });
                }
            }
            if (distances.length > 0) {
                distances.sort((a, b) => a.distance - b.distance);
                return distances[0].elevatorIndex;
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

        continueExecutingCall() {
            for(let i in this.executingCalls) {
                let elevatorIndex = this.executingElevators[i]
                let call = this.executingCalls[i]

                if (elevatorIndex && !this.areElevatorsAtLevel(call)) {
                    this.$refs.elevators[elevatorIndex].initNewCall(call);
                    
                }
                else if (elevatorIndex && this.areElevatorsAtLevel(call)) {
                    return this.$parent.removeFromMainQueue(call);
                }

            }
        },

        handleNewCall(call) {
            let elevatorIndex = this.getElevatorIndex(call);
            if (elevatorIndex && !this.areElevatorsAtLevel(call)) {
                this.executingCalls.push(call);
                this.executingElevators.push(elevatorIndex);

                if (this.pendingCalls.length > 0) {
                    this.pendingCalls.shift();
                }

                this.$refs.elevators[elevatorIndex].initNewCall(call);
            } else if (elevatorIndex && this.areElevatorsAtLevel(call)) {
                return this.$parent.removeFromMainQueue(call);
            }
            else if (!elevatorIndex) {
                this.addToQueue(call);
            }
        },

        removeFromExecutingQueue(finishedCall) {
            if (this.executingCalls.length === 1) {
                this.executingCalls.shift();
                this.executingElevators.shift()
            } else {
                this.executingCalls = this.executingCalls.filter((call) => {
                    return call !== finishedCall;
                });
                
            }
        },

        nextCall(currentCall) {
            this.$parent.removeFromMainQueue(currentCall);
            this.removeFromExecutingQueue(currentCall);

            if (this.pendingCalls.length > 0) {
                this.handleNewCall(this.pendingCalls[0]);
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

        if (localStorage.executingElevators) {
            this.executingElevators = JSON.parse(
                localStorage.getItem("executingElevators")
            );
        }

        if (localStorage.executingCalls) {
            
            this.executingCalls = JSON.parse(
                localStorage.getItem("executingCalls")
            );

            if (this.executingCalls.length > 0 && this.executingElevators.length > 0) {
                this.continueExecutingCall()
            }
        }
    }

};
</script>

<style scoped>
.all-elevators-container {
    display: flex;
}
</style>
