<script setup>
const props = defineProps({
  dice: Object,
})

const emit = defineEmits(['holdThisDice'])
</script>

<template>
  <div className="dice" :style="{ 'background-color': dice.isHeld ? '#59E391' : 'white' }" @click="emit('holdThisDice')">
    <h2>{{dice.value}}</h2>
  </div>
</template>

<style>

</style>
