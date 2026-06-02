<script setup lang="ts">
defineProps<{
  background?: string
}>()
</script>

<template>
  <div
    class="slidev-layout section"
    :style="background ? { backgroundColor: background } : {}"
  >
    <div class="section-background-text">
      <slot name="background-text" />
    </div>
    <div class="section-content">
      <slot />
    </div>
  </div>
</template>

<style>
.slidev-layout.section {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: rgb(10, 10, 10);
  position: relative;
}

.section-background-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.section-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
}

.section-content .font-eyebrow-title {
  position: static;
}

.section-content .font-title-section {
  text-align: center;
}

.section-content .font-tagline {
  text-align: center;
}
</style>
