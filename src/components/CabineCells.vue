<template>
    <div class="container">
        <div v-for="levelNumber in totalLevels" :key="levelNumber">
            <SingleCabineCell 
                :direction="direction" 
                :elevalorPosition="elevalorPosition" 
                :flexOrder="levelNumber"
                :finishedCallAnimation="finishedCallAnimation"
             />
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
    props: ['totalLevels', 'elevalorPosition', 'direction', 'finishedCallAnimation'],
};
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column-reverse;
    flex-wrap: wrap;
}

</style>
