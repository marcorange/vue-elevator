<template>
    <div class="container">
        <CabineCells
            :elevalorPosition="currentLevel"
            :totalLevels="numberOfLevels"
        />

        <CallButtons :call="callElevator" :totalLevels="numberOfLevels" />
    </div>
</template>

<script>
import CabineCells from "./components/CabineCells";
import CallButtons from "./components/CallButtons";


export default {
    name: "App",
    data() {
        return {
            currentLevel: 1,
            numberOfLevels: 7,
        };
    },
    components: {
        CabineCells,
        CallButtons
    },
    computed: {
        computedTop: function () {
            return this.currentLevel;
        },
    },
    methods: {
        callElevator(destLevel) {
            let interval = setInterval(() => {
                if (this.currentLevel === destLevel) {
                    clearInterval(interval);
                    return;
                }
                else if (this.currentLevel < destLevel) {
                    this.currentLevel += 1;
                }
                else {
                  this.currentLevel -= 1;
                }
            }, 1000);
        },
    },
};
</script>

<style>
.container {
    display: flex;
}
</style>
