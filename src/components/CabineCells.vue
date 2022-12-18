<template>
    <div class="container">
        <div v-for="levelNumber in totalLevels" :key="levelNumber">
            <SingleCabineCell :elevalorPosition="elevalorPosition" :flexOrder="levelNumber" />
        </div>
    </div>
</template>

<script>
import SingleCabineCell from "./SingleCabineCell";
export default {
    name: "CabineCells",
    components: {
        SingleCabineCell,
    },
    props: ['totalLevels', 'elevalorPosition'],
};
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column-reverse;
    flex-wrap: wrap;
}

.elevator {
    width: 100px;
    height: 100px;
    background-color: blue;
    margin: 10px;
    position: relative;
}
</style>
