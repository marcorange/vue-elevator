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

import AllElevators from "./components/AllElevators";
import CallButtons from "./components/CallButtons";
/* eslint-disable no-unused-vars */

export default {
    name: "App",
    data() {
        return {
            numberOfLevels: 5,
            numberOfElevators: 3,

            executionQueue: [],
        };
    },
    components: {
        AllElevators,
        CallButtons,
    },
    // watch: {
    //     currentLevel() {
    //         localStorage.currentLevel = this.currentLevel;
    //     },

    //     executionQueue: {
    //         deep: true,

    //         handler() {
    //             localStorage.setItem(
    //                 "executionQueue",
    //                 JSON.stringify(this.executionQueue)
    //             );
    //         },
    //     },
    // },
    methods: {
        // resetValues() {
        //     //start params
        // },

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

    // mounted() {
    //     if (localStorage.currentLevel) {
    //         this.currentLevel = Number(localStorage.currentLevel);
    //     }

    //     if (localStorage.executionQueue) {
    //         this.executionQueue = JSON.parse(
    //             localStorage.getItem("executionQueue")
    //         );
    //         this.executionQueue.unshift(this.executionQueue[0]);
    //         this.nextCall();
    //     }
    // },
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
