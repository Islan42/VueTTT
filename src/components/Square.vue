<script setup>
const props = defineProps(['value'])
const emit = defineEmits(['square-click'])
</script>

<template>
  <button class="square" @click="$emit('square-click')">{{value}}</button>
</template>