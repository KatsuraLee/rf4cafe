<template>
  <div class="relative h-screen overflow-hidden flex flex-col">
    <!-- 顶部导航 -->
    <header class="h-12 bg-gray-900 text-white flex items-center px-6 flex-shrink-0">
      <h1 class="font-bold text-lg">咖啡厅</h1>
      <nav class="ml-auto">
        <ul class="flex space-x-4">
          <li><a href="#" class="hover:text-blue-300">订单</a></li>
        </ul>
      </nav>
    </header>

    <!-- 主体内容 -->
    <div class="relative flex flex-1">
      <!-- 左侧菜单 -->
      <nav class="w-60 p-4 flex flex-col space-y-3 select-none bg-gray-800 text-white z-10">
        <h2 class="text-lg font-bold mb-4">水塘</h2>
        <ul>
          <li v-for="(waterBody, index) in waterBodies" :key="index" @click="selectWaterBody(index)">
            <span :class="{ 'text-blue-400': activeWaterBodyIndex === index }">{{ waterBody.name }}</span>
          </li>
        </ul>
      </nav>

      <!-- 右侧内容 -->
      <main class="flex-1 p-4 relative z-10">
        <!-- 背景图 -->
        <div
          class="absolute top-0 left-0 w-full h-full bg-cover bg-center filter blur-2xl scale-110 z-0"
          :style="{ backgroundImage: `url(${selectedWaterBody.background})` }"
        ></div>

        <!-- 鱼类卡片 -->
        <div class="grid grid-cols-3 gap-4 mt-10">
          <div v-for="(fish, index) in selectedWaterBody.fishes" :key="index" class="bg-gray-800 rounded-lg p-4 relative">
            <img :src="fish.image" alt="鱼" class="w-full h-20 object-cover mb-2">
            <div class="flex justify-between items-center">
              <div>
                <h3 class="text-white">{{ fish.name }}</h3>
                <p class="text-sm text-gray-400">{{ fish.location }}</p>
              </div>
              <div class="bg-blue-500 text-white rounded-full w-8 h-8 flex items-center justify-center">
                {{ fish.id }}
              </div>
            </div>
            <div class="mt-2 border-t border-gray-600 pt-2">
              <p class="text-sm text-gray-400">数量 {{ fish.quantity }} 个</p>
              <p class="text-sm text-gray-400">质量 {{ fish.weight }} 克以上</p>
              <p class="text-sm text-gray-400 mt-2"><span class="mr-1">¥</span>{{ fish.price }}</p>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const waterBodies = [
  {
    name: '克马玲诺也湖',
    background: '/path/to/background1.jpg',
    fishes: [
      {
        id: '7636',
        name: '银鲫',
        image: '/path/to/fish1.jpg',
        location: '克马玲诺也湖',
        quantity: 3,
        weight: 180,
        price: 3.11
      },
      // 其他鱼类...
    ]
  },
  // 其他水塘...
]

const activeWaterBodyIndex = ref(0)

const selectedWaterBody = computed(() => waterBodies[activeWaterBodyIndex.value])

function selectWaterBody(index: number) {
  activeWaterBodyIndex.value = index
}
</script>

<style scoped>
/* 顶部导航 */
header {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* 左侧菜单 */
nav ul li {
  cursor: pointer;
  padding: 0.5rem 0;
}

nav ul li:hover {
  text-decoration: underline;
}

/* 右侧内容 */
main {
  background-color: rgba(0, 0, 0, 0.5);
}

main .grid {
  margin-top: 5rem;
}

main .grid div {
  position: relative;
  overflow: hidden;
}

main .grid div::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1;
}

main .grid div img {
  z-index: 2;
  position: relative;
}

main .grid div .bg-blue-500 {
  z-index: 3;
  position: relative;
}

main .grid div .text-white {
  z-index: 3;
  position: relative;
}

main .grid div .text-sm {
  z-index: 3;
  position: relative;
}

/* 动画 */
.slide-fade-enter-active,
.slide-fade-leave-active {
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
