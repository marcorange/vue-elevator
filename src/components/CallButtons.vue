<template>
    <div class="buttons-container">
        <div v-for="level in numberOfLevels" :key="level">
            <SingleCallButton @click="addToQueueClick(level)" 
            :executionQueue="executionQueue"
            :executingCalls="executingCalls"
            :level="level"/>
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
        
        props: ['numberOfLevels', 'addToQueueClick', 'executionQueue', 'executingCalls']
    }
</script>

<style>
.buttons-container {
    display: flex;
    flex-direction: column-reverse;
}
</style>