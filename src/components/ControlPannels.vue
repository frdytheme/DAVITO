<script setup>
import { ref } from "vue";
import EditData from "./pages/EditData.vue";
const isResize = ref(false);
const currentWidth = ref(600);
const startX = ref(0);
const startWidth = ref(0);
function resizeOn(e) {
  isResize.value = true;
  startX.value = e.clientX;
  startWidth.value = currentWidth.value;

  window.addEventListener("mousemove", resizePannel);
  window.addEventListener("mouseup", resizeOff);
}
function resizePannel(e) {
  if (!isResize.value) return;

  const moveX = e.clientX - startX.value;
  currentWidth.value = startWidth.value + moveX;
}
function resizeOff(e) {
  isResize.value = false;
  window.removeEventListener("mousemove", resizePannel);
  window.removeEventListener("mouseup", resizeOff);
}
</script>

<template>
    <div class="control-box" :style="{ width: currentWidth + 'px' }">
      <EditData />
      <div class="resizeLine" @mousedown="resizeOn" @mouseup="resizeOff">크기조절라인</div>
    </div>
</template>

<style>
.control-box {
  height: 100%;
  grid-column: 2 / 2;
  position: relative;
  overflow: hidden;
  display: none;
}
.box-wrapper .control-box {
  height: 100%;
}
.control-box .resizeLine {
  width: 5px;
  height: 100%;
  position: absolute;
  top: 0;
  right: 0;
  background-color: var(--color-orange);
  cursor: ew-resize;
  opacity: 0;
  transition: 0.3s;
  text-indent: -9999px;
}
.control-box .resizeLine:hover {
  opacity: 1;
}
.control-box.edit_note,
.control-box.display_settings,
.control-box.settings {
  display: block;
}
</style>
