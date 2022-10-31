<!--
 * @Author: OCEAN.GZY
 * @Date: 2022-10-28 15:31:23
 * @LastEditors: OCEAN.GZY
 * @LastEditTime: 2022-10-31 10:43:34
 * @FilePath: /echarts/ts-echarts/vueapp/vueecharts/src/App.vue
 * @Description: 注释信息
-->
<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'
import EchartdemoVue from './components/Echartdemo.vue';
</script>

<template>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div> -->
  <!-- <HelloWorld msg="Vite + Vue" /> -->
<div class="main">
  <EchartdemoVue></EchartdemoVue>
</div>

</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
.main{
  width: 100%;
  height: 100vh;  
  /* background-color:blue; */
}
</style>
