<template>
    <div class="buttons-container">
        <div v-for="levelNumber in totalLevels" :key="levelNumber">
            <SingleCallButton @click="click(levelNumber)" :levelNumber="levelNumber" 
            :buttonQueue="buttonQueue"
            :id="levelNumber"/>
        </div>
    </div>
</template>

<script>
import SingleCallButton from "./SingleCallButton";
    export default {
        name: 'CallButtons',
        components: {
            SingleCallButton
        },
        props: ['totalLevels', 'click', 'buttonQueue']
    }
</script>

<style>
.buttons-container {
    display: flex;
    flex-direction: column-reverse;
}
</style>