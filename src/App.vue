<template>
    <div v-cloak>
        <button @click="resetValues">RESET VALUES</button>
        <div class="main-container">
            <AllElevators
                ref="allElevators"
                :numberOfElevators="numberOfElevators"
                :numberOfLevels="numberOfLevels"
                :executionQueue="executionQueue"
            />

            <CallButtons
                :click="addToQueue"
                :totalLevels="numberOfLevels"
                :buttonQueue="executionQueue"
            />
        </div>
    </div>
</template>

<script>
import { numberOfLevels, numberOfElevators } from "./config.js"
import AllElevators from "./components/AllElevators";
import CallButtons from "./components/CallButtons";
/* eslint-disable no-unused-vars */

export default {
    name: "App",
    data() {
        return {
            numberOfLevels: numberOfLevels,
            numberOfElevators: numberOfElevators,

            executionQueue: [],
        };
    },
    components: {
        AllElevators,
        CallButtons,
    },
    watch: {
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
            localStorage.clear();
            window.location.reload();
        },

        removeFromMainQueue(finishedCall) {
            if (this.executionQueue.length === 1) {
                this.executionQueue.shift();
            } else {
                this.executionQueue = this.executionQueue.filter((call) => {
                    return call !== finishedCall;
                })
            }

        },

        addToQueue(destinationLevel) {

            if (!this.executionQueue.includes(destinationLevel)) {
                this.executionQueue.push(destinationLevel);

                if (this.executionQueue.length <= this.numberOfElevators) {
                    this.$refs.allElevators.handleCall(destinationLevel);
                }
                else {
                    this.$refs.allElevators.addToQueue(destinationLevel);
                }
            }
        },
    },

    mounted() {
        if (localStorage.executionQueue) {
            this.executionQueue = JSON.parse(
                localStorage.getItem("executionQueue")
            );
        }
    },
};
</script>

<style scoped>
[v-cloak] {
    display: none;
}

button {
    font-weight: bold;
    font-size: 0.7rem;
    background-color: rgb(11, 194, 11);
    border-style: none;
    border-radius: 100%;
    width: 4rem;
    height: 2rem;
    margin-left: 3%;
    margin-bottom: 3%;
}

.main-container {
    display: flex;
}
</style>
