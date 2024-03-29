<script setup lang="ts">
console.log('Child component setup')
defineProps({
  message: String
})

const emit = defineEmits(['response'])

setTimeout(() => {
  emit('response', 'Hello from child component')
}, 1000)
</script>

<template>
  <p class="paragraph">{{ message }}</p>
  <slot>Fallback</slot>
</template>

<style>
.paragraph {
  color: red;
}
</style>
