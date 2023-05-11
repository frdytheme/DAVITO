<script setup>
import { ref } from "vue";
import ControlPannels from "./ControlPannels.vue";

const googleIcon = "material-symbols-outlined";
const isShow = ref("");
let id = 1;
const menus = [
  { id: id++, icon: "edit_note", title: "데이터베이스", des: "데이터를 추가/제거하거나 수정합니다" },
  { id: id++, icon: "display_settings", title: "차트스타일", des: "차트 스타일을 변경합니다" },
  { id: id++, icon: "settings", title: "환경설정", des: "사용자에 맞춰 환경을 설정합니다" },
];

function checkIcon(menu) {
  isShow.value === menu ? (isShow.value = "") : (isShow.value = menu);
  const icons = document.querySelectorAll(".snb li span");
  icons.forEach((icon) => {
    icon.style.borderRight = "none";
    isShow.value === icon.id ? (icon.style.borderRight = "3px solid var(--color-green)") : false;
  });
}
</script>

<template>
  <div class="side-box">
    <ul class="snb">
      <li v-for="menu in menus" :key="menu.id">
        <span :class="googleIcon" @click="checkIcon(menu.icon)" :id="menu.icon">{{ menu.icon }}</span>
        <div class="snb-side">
          <h2>{{ menu.title }}</h2>
          <p>{{ menu.des }}</p>
        </div>
      </li>
    </ul>
  </div>
  <ControlPannels :class="isShow" />
</template>

<style>
.side-box .snb {
  width: 70px;
  height: 100%;
  background-color: var(--color-white);
}
.side-box li {
  height: 100px;
  text-align: center;
  padding: 15px 0;
  cursor: pointer;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}
.side-box li span {
  width: 100%;
  height: 100%;
  position: absolute;
  font-size: 35px;
  opacity: 0.8;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
  color: var(--color-blk);
}
.side-box li span:hover {
  animation: mouseOn 0.3s cubic-bezier(0.49, 0.06, 0.08, 1) forwards;
  animation-play-state: running;
  font-size: 40px;
  border-right: 3px solid var(--color-green);
}
@keyframes mouseOn {
  50% {
    font-size: 50px;
  }
  100% {
    font-size: 45px;
    opacity: 1;
  }
}
.snb-side {
  width: 280px;
  height: 100%;
  padding: 15px;
  background-color: var(--color-white);
  position: absolute;
  top: 0;
  left: 100%;
  text-align: left;
  display: none;
  border-radius: 0 10px 10px 0;
  z-index: 99;
}
.snb-side h2 {
  font-size: 30px;
  font-weight: bold;
  margin-bottom: 5px;
}
.snb-side p {
  font-size: 14px;
}
.side-box li span:hover + .snb-side {
  display: block;
}
</style>
