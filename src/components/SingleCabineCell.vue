<template>
    <div
        v-if="flexOrder === elevatorPosition"
        :class="finishedCallAnimation ? 'cell animated' : 'cell'"
        :style="{ order: flexOrder, 'background-color': '#ff4800' }"
    >
        {{ movingDirection.level }}
        {{ movingDirection.direction }}
    </div>
    <div v-else class="cell" :style="{ order: flexOrder }">
    </div>
</template>

<script>
export default {
    name: "SingleCabineCell",
    props: ['flexOrder', 'elevatorPosition', 'movingDirection', 'finishedCallAnimation'],
};
</script>

<style>
.cell {
    min-width: 4rem;
    min-height: 2rem;
    background-color: #ff6f0083;
    margin: 11px;
    position: relative;
    border-radius: 10%;
    text-align: center;
    color: #540375;
    font-weight: bold;
}

.animated {
    animation-name: callFinished;
    animation-duration: 1s;
    animation-iteration-count: 3;
}

@keyframes callFinished {
    0% {background-color: #ff4800;}
    50% {background-color: #ffbf00aa;}
    100% {background-color: #ff4800;}
}

</style>


