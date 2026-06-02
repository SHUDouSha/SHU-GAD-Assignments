<template>
  <div
    :class="['component-image-player', customClass]"
  >
    <img :src="src" />
  </div>
</template>

<script setup lang="ts">
interface Props {
  src: string
  customClass?: string
}

withDefaults(defineProps<Props>(), {
  customClass: ''
})
</script>

<style>
.component-image-player {
  position: absolute;
}

.component-image-player img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
</style>
