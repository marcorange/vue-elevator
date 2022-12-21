<template>
    <button @click="resetValues">RESET VALUES</button>
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
            :buttonQueue="executionQueue"
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
            executionQueue: [],
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
    watch: {
        currentLevel() {
            localStorage.currentLevel = this.currentLevel;
        },

        executionQueue: {
            deep: true,

            handler() {
                localStorage.setItem(
                    "executionQueue",
                    JSON.stringify(this.executionQueue)
                );
            },
        },
    },
    methods: {

        resetValues() {
          //later defaults or start params
          this.currentLevel = 1;
          this.executionQueue = [];
          this.movingDirection.level = null;
          this.movingDirection.direction = "🟢";
          this.finishedCallAnimation = false;
        },


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

            this.executionQueue.shift();

            if (this.executionQueue.length > 0) {
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
                this.movingDirection.level = this.executionQueue[0];
                if (this.currentLevel === this.executionQueue[0]) {
                    this.finishCall(interval);
                } else if (this.currentLevel < this.executionQueue[0]) {
                    this.movingDirection.direction = "⬆";
                    this.currentLevel += 1;
                } else if (this.currentLevel > this.executionQueue[0]) {
                    this.movingDirection.direction = "⬇";
                    this.currentLevel -= 1;
                } else {
                    return;
                }
            }, 1000);
        },

        addToQueue(destLevel) {
            if (!this.executionQueue.includes(destLevel)) {
                if (
                    destLevel !== this.currentLevel ||
                    (destLevel === this.currentLevel &&
                        this.movingDirection.level)
                ) {
                    this.executionQueue.push(destLevel);
                    if (this.executionQueue.length === 1) {
                        this.initNewCall();
                    }
                }
            }
        },
    },

    mounted() {
        if (localStorage.currentLevel) {
            this.currentLevel = Number(localStorage.currentLevel);
        }

        if (localStorage.executionQueue) {
            this.executionQueue = JSON.parse(
                localStorage.getItem("executionQueue")
            );
            this.executionQueue.unshift(this.executionQueue[0]);
            this.nextCall();
        }
    },
};
</script>

<style scoped>
button {
  font-weight: bold;
  font-size: 0.7rem;
  background-color: rgb(11, 194, 11);
  border-style: none;
  border-radius: 100%;
  width: 4rem;
  height: 2rem;
  margin-left: 3%;
  margin-bottom: 3%
}

.container {
    display: flex;
}
</style>
