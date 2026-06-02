<template>
  <div
    :class="['component-video-player', customClass]"
  >
    <video :src="src" :autoplay="autoplay" :controls="controls" :loop="loop" />
  </div>
</template>

<script setup lang="ts">
interface Props {
  src: string
  autoplay?: boolean
  controls?: boolean
  loop?: boolean
  customClass?: string
}

withDefaults(defineProps<Props>(), {
  autoplay: false,
  controls: true,
  loop: false,
  customClass: ''
})
</script>

<style>
.component-video-player {
  position: absolute;
}

.component-video-player video {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
</style>
