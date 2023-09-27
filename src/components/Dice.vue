<script setup>
import { ref } from 'vue'

const props = defineProps({
  value: String | Number,
  isHeld: Boolean
})

const emit = defineEmits(['holdDice'])

const styleObject = ref({
  backgroundColor: props.isHeld ? "#59E391" : "white"
})

console.log("(props.isHeld", props.isHeld)
</script>

<template>
  <div className="dice" :style="styleObject" @click="emit('holdDice')">
    <h2>{{value}}</h2>
  </div>
</template>

<style>

</style>
