<template>
    <div
        class="cell"
        v-if="flexOrder === elevalorPosition"
        :style="{ order: flexOrder, 'background-color': 'green' }"
    >
        {{ flexOrder }}
        {{elevalorPosition}}
    </div>
    <div class="cell" v-else :style="{ order: flexOrder }">
        {{ flexOrder }}
    </div>
</template>

<script>
export default {
    name: "SingleCabineCell",
    props: ["flexOrder", "elevalorPosition"],
};
</script>

<style>
.cell {
    min-width: 4rem;
    min-height: 4rem;
    background-color: bisque;
    margin: 5px;
    position: relative;
    border-radius: 10%;
}
</style>
