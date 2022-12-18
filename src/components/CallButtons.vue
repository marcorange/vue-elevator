<template>
    <div class="container">
        <div v-for="levelNumber in totalLevels" :key="levelNumber">
            <SingleCallButton @click="call(levelNumber)" :levelNumber="levelNumber"/>
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
        props: ['totalLevels', 'call']
    }
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column-reverse;
}
</style>