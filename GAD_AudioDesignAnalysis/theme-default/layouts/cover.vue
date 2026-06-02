<script setup lang="ts">
defineProps<{
  background?: string
}>()
</script>

<template>
  <div
    class="slidev-layout cover"
    :style="background ? { backgroundColor: background } : {}"
  >
    <div class="cover-background-text">
      <slot name="background-text" />
    </div>
    <div class="cover-content">
      <slot />
    </div>
  </div>
</template>

<style>
.slidev-layout.cover {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  height: 100%;
  background-color: rgb(10, 10, 10);
  position: relative;
}

.cover-background-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.cover-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-left: 144px;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
}

.cover-content .font-eyebrow-title {
  position: static;
}

.cover-content .font-title-master {
  margin-top: 0;
}

.cover-content .font-tagline {
  margin-top: 0;
}
</style>
