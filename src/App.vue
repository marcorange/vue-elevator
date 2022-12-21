<template>
    <div class="container">
        <CabineCells
            :elevalorPosition="currentLevel"
            :totalLevels="numberOfLevels"
            :direction="movingDirection"
            :finishedCallAnimation="finishedCallAnimation"
        />

        <CallButtons
            :click="addToQueue"
            :totalLevels="numberOfLevels"
            :buttonQueue="executeQueue"
        />
    </div>
</template>

<script>
import CabineCells from "./components/CabineCells";
import CallButtons from "./components/CallButtons";
/* eslint-disable no-unused-vars */

export default {
    name: "App",
    data() {
        return {
            currentLevel: 1,
            numberOfLevels: 5,
            executeQueue: [],
            movingDirection: {
                level: null,
                direction: "🟢",
            },
            finishedCallAnimation: false,
        };
    },
    components: {
        CabineCells,
        CallButtons,
    },
    methods: {
        displayElevatorState(isReady) {
            this.movingDirection.level = null;

            if (isReady) {
                this.finishedCallAnimation = false;
                this.movingDirection.direction = "🟢";
            } else {
                this.finishedCallAnimation = true;
                this.movingDirection.direction = "🔴";
            }
        },

        nextCall() {
            this.displayElevatorState(true);

            this.executeQueue.shift();

            if (this.executeQueue.length > 0) {
                console.log(this.executeQueue[0]);
                return this.initNewCall();
            } else {
                return;
            }
        },

        finishCall(interval) {
            clearInterval(interval);

            this.displayElevatorState(false);

            setTimeout(this.nextCall, 3000);
        },

        initNewCall() {
            let interval = setInterval(() => {
                this.movingDirection.level = this.executeQueue[0];
                if (this.currentLevel === this.executeQueue[0]) {
                    this.finishCall(interval);
                } else if (this.currentLevel < this.executeQueue[0]) {
                    this.movingDirection.direction = "⬆";
                    this.currentLevel += 1;
                } else if (this.currentLevel > this.executeQueue[0]) {
                    this.movingDirection.direction = "⬇";
                    this.currentLevel -= 1;
                } else {
                    return;
                }
            }, 1000);
        },

        addToQueue(destLevel) {
            if (
                !this.executeQueue.includes(destLevel) &&
                destLevel !== this.currentLevel
            ) {
                this.executeQueue.push(destLevel);
                if (this.executeQueue.length === 1) {
                    this.initNewCall();
                }
            }
        },
    },
};
</script>

<style>
.container {
    display: flex;
}
</style>
