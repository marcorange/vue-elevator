<template>
    <button>{{levelNumber}}</button>
</template>

<script>
    export default {
        name: 'SingleCallButton',
        props: ['levelNumber']
    }    
</script>

<style scoped>
button {
    min-width: 3rem;
    min-height: 3rem;
    background-color: red;
    margin: 15px 5px 11px;
    border-radius: 50%;
}
</style>
