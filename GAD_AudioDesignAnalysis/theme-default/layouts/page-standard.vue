<script setup lang="ts">
defineProps<{
  background?: string
}>()
</script>

<template>
  <div
    class="slidev-layout page-standard"
    :style="background ? { backgroundColor: background } : {}"
  >
    <div class="page-standard-background-text">
      <slot name="background-text" />
    </div>
    <div class="page-standard-content">
      <slot />
    </div>
  </div>
</template>

<style>
.slidev-layout.page-standard {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: rgb(10, 10, 10);
  position: relative;
}

.page-standard-background-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.page-standard-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  padding-top: 300px;
  padding-bottom: 113px;
  padding-left: 144px;
  padding-right: 144px;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  gap: 0;
}
</style>
