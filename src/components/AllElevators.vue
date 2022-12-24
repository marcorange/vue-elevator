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
            pendingCalls: []
        };
    },
    components: {
        SingleElevator,
    },

    props: ["numberOfElevators", "numberOfLevels", "executionQueue"],
    methods: {

        getFreeElevator() {
            for (let i in this.$refs.elevators) {
                if (!this.$refs.elevators[i].elevatorIsBusy()) {
                    return this.$refs.elevators[i];
                }
            }
            return;
        },

        addToQueue(call) {
            this.pendingCalls.push(call)
        },

        areElevatorsAtLevel(level) {
            for (let i in this.$refs.elevators) {
                if (level === this.$refs.elevators[i].getPosition()
                && !this.$refs.elevators[i].elevatorIsBusy()) {
                    return true;
                }
            }
            return false;
        },

        handleCall(call) {
            let elevator = this.getFreeElevator();

            if (elevator && !this.areElevatorsAtLevel(call)) {
                this.executingCalls.push(call);
                if (this.pendingCalls.length > 0) {
                    this.pendingCalls.shift();
                }
                elevator.initNewCall(call)
            }
            else if (elevator && this.areElevatorsAtLevel(call)) {
                return this.$parent.removeFromMainQueue(call);
            }
            else if (!elevator) {
                this.addToQueue(call)
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
            this.$parent.removeFromMainQueue(currentCall)
            this.removeFromExecuting(currentCall);

            
            if (this.pendingCalls.length > 0) {
                this.handleCall(this.pendingCalls[0]);
            } else {
                return;
            }
        },
    },
};
</script>

<style scoped>
.all-elevators-container {
    display: flex;
}
</style>
