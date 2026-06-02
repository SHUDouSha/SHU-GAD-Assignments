<script setup lang="ts">
defineProps<{
  background?: string
}>()
</script>

<template>
  <div
    class="slidev-layout page-image-text"
    :style="background ? { backgroundColor: background } : {}"
  >
    <div class="page-image-text-background-text">
      <slot name="background-text" />
    </div>
    <div class="page-image-text-content">
      <slot />
    </div>
  </div>
</template>

<style>
.slidev-layout.page-image-text {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: rgb(10, 10, 10);
  position: relative;
}

.page-image-text-background-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.page-image-text-content {
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

.page-image-text-content .component-image-player {
  position: absolute;
  top: 299px;
  left: 144px;
  width: 1075px;
  height: 667px;
  box-sizing: border-box;
}

.page-image-text-content .container-textbox-plain {
  position: absolute;
  top: 299px;
  left: 1239px;
  width: 537px;
  height: 667px;
  box-sizing: border-box;
}
</style>
