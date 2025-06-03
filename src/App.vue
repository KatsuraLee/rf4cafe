<template>
  <div class="relative h-screen overflow-hidden flex flex-col">
    <!-- 顶部导航 -->
    <header class="h-12 bg-gray-900 text-white flex items-center px-6 flex-shrink-0">
      <h1 class="font-bold text-lg">RF4 Vue Site</h1>
    </header>

    <!-- 主体内容 -->
    <div class="relative flex flex-1">
      <!-- 背景图 -->
      <div
        class="absolute top-0 left-0 w-full h-full bg-cover bg-center filter blur-2xl scale-110 z-0"
        :style="{ backgroundImage: `url(${navItems[activeIndex].background})` }"
      ></div>

      <!-- 左侧菜单 -->
      <nav class="w-60 p-4 flex flex-col space-y-3 select-none z-10">
        <div
          v-for="(item, index) in navItems"
          :key="index"
          @click="switchIndex(index)"
          :class="[
            'cursor-pointer transition-colors duration-200',
            activeIndex === index ? 'text-blue-400 font-semibold' : 'text-white hover:text-blue-300'
          ]"
        >
          {{ item.label }}
        </div>
      </nav>

      <!-- 右侧banner -->
      <main class="flex-1 flex justify-center items-center relative z-10">
        <transition name="slide-fade" mode="out-in">
          <img
            :key="activeIndex"
            :src="navItems[activeIndex].banner"
            alt="banner"
            class="max-h-[300px] rounded-lg shadow-lg object-contain"
          />
        </transition>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const navItems = [
  { label: '选项1', banner: '/cafe/banner1.png', background: '/cafe/bg1.png' },
  { label: '选项2', banner: '/cafe/banner2.png', background: '/cafe/bg2.png' },
  { label: '选项3', banner: '/cafe/banner3.png', background: '/cafe/bg3.png' },
  { label: '选项4', banner: '/cafe/banner4.png', background: '/cafe/bg4.png' },
  { label: '选项5', banner: '/cafe/banner5.png', background: '/cafe/bg5.png' },
  { label: '选项6', banner: '/cafe/banner6.png', background: '/cafe/bg6.png' },
  { label: '选项7', banner: '/cafe/banner7.png', background: '/cafe/bg7.png' },
  { label: '选项8', banner: '/cafe/banner8.png', background: '/cafe/bg8.png' },
  { label: '选项9', banner: '/cafe/banner9.png', background: '/cafe/bg9.png' },
  { label: '选项10', banner: '/cafe/banner10.png', background: '/cafe/bg10.png' },
  { label: '选项11', banner: '/cafe/banner11.png', background: '/cafe/bg11.png' },
  { label: '选项12', banner: '/cafe/banner12.png', background: '/cafe/bg12.png' },
  { label: '选项13', banner: '/cafe/banner13.png', background: '/cafe/bg13.png' },
  { label: '选项14', banner: '/cafe/banner14.png', background: '/cafe/bg14.png' },
  { label: '选项15', banner: '/cafe/banner15.png', background: '/cafe/bg15.png' },
  { label: '选项16', banner: '/cafe/banner16.png', background: '/cafe/bg16.png' },
]

const activeIndex = ref(0)
function switchIndex(index: number) {
  activeIndex.value = index
}
</script>

<style>
.slide-fade-enter-active, .slide-fade-leave-active {
  transition: all 0.3s ease;
  position: absolute;
  width: 100%;
}
.slide-fade-enter-from {
  transform: translateX(100%);
  opacity: 0;
}
.slide-fade-enter-to {
  transform: translateX(0);
  opacity: 1;
}
.slide-fade-leave-from {
  transform: translateX(0);
  opacity: 1;
}
.slide-fade-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}
</style>
