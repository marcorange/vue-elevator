<template>
    <button 
        :class="buttonQueue[0] === id ? 'is-next' : ''"
        :style="buttonQueue.includes(id) ? { 'background-color': 'rgb(11, 194, 11)'} : ''">{{levelNumber}}
    </button>
</template>

<script>
    export default {
        name: 'SingleCallButton',
        props: ['levelNumber', 'buttonQueue', 'id']
    }    
</script>

<style scoped>
button {
    min-width: 2rem;
    min-height: 2rem;
    background-color: rgb(198, 197, 197);
    margin: 11px 15px;
    border-radius: 50%;
    border-style: none;
    font-weight: bold;
}

.in-queue {
    background-color: rgb(11, 194, 11);
}

.is-next {
    animation-name: next;
    animation-duration: 1s;
    animation-iteration-count: infinite;
}

@keyframes next {
    0% {background-color: rgb(11, 194, 11);}
    50% {background-color: rgb(171, 169, 169);}
    100% {background-color: rgb(11, 194, 11);}
}

</style>
