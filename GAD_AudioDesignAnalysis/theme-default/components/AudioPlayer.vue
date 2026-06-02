<template>
  <div
    :class="['component-audio-player', customClass]"
  >
    <audio :src="src" :autoplay="autoplay" :controls="controls" :loop="loop" />
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
.component-audio-player {
  position: absolute;
}

.component-audio-player audio {
  width: 100%;
  height: 100%;
}
</style>
