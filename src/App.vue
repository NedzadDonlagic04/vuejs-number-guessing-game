<script setup>
import { ref } from 'vue';

const count = ref(0);

</script>

<template>
    <p>{{count}}</p>
    <button @click="count++">Increment</button>
</template>

<style scoped>
    p {
        color: red;
    }

    button {
        cursor: pointer;
    }
</style>