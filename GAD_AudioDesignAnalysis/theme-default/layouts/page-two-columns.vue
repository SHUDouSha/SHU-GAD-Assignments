<script setup lang="ts">
defineProps<{
  background?: string
}>()
</script>

<template>
  <div
    class="slidev-layout page-two-columns"
    :style="background ? { backgroundColor: background } : {}"
  >
    <div class="page-two-columns-background-text">
      <slot name="background-text" />
    </div>
    <div class="page-two-columns-content">
      <slot />
    </div>
  </div>
</template>

<style>
.slidev-layout.page-two-columns {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: rgb(10, 10, 10);
  position: relative;
}

.page-two-columns-background-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.page-two-columns-content {
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

.page-two-columns-content .container-textbox-plain {
  position: absolute;
  top: 299px;
  left: 144px;
  width: 806px;
  height: 667px;
  box-sizing: border-box;
}

.page-two-columns-content .container-textbox-gray-border {
  position: absolute;
  top: 299px;
  left: 970px;
  width: 806px;
  height: 667px;
  box-sizing: border-box;
  border-left: 1px solid rgb(212, 90, 42);
}
</style>
