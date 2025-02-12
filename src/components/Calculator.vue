<template>
    <div class="px-2" style="max-width: 400px; margin: 40px auto; background: #234">
        <div class="w-full rounded-sm mt-1 mb-1 px-2 text-right font-bold text-white">
            {{ calculatorValue || 0 }}
        </div>

        <div class="row"></div>
    </div>


</template>

<script setup>
import { defineprops } from 'vue'
import { ref } from 'vue'

// const data = ref{ []}
calculatorValue: ''
calculatorElements: ['C', '*', '/', '-', '7', '8', '9', '+', '4', '5', '6', '%', '1', '2', '3', '=', '0', '.']
</script>

<style scoped></style>