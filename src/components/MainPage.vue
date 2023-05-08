<script setup>
import MainNav from "./MainNav.vue";
import MainView from "./MainView.vue";
import SideNav from "./SideNav.vue";
</script>

<template>
  <main>
    <SideNav />
    <MainView />
  </main>
</template>

<style>
main {
  width: 100%;
  height: 100vh;
  display: grid;
  grid-template-columns: auto auto 1fr;
  grid-auto-rows: 1fr;
}
</style>
