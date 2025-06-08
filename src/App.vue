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
        <!-- 鱼类卡片 -->
        <img
          :src="selectedWaterBody.fishImage"
          alt="鱼类卡片"
          class="w-full h-full object-contain mt-10"
        />
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const waterBodies = [
  {
    name: '克马玲诺也湖',
    fishImage: '/path/to/fishCards_kema.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '惟有诺克河',
    fishImage: '/path/to/fishCards_weinuo.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '旧奥斯特罗格湖',
    fishImage: '/path/to/fishCards_jiuostero.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '白河',
    fishImage: '/path/to/fishCards_baihe.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '廓里湖',
    fishImage: '/path/to/fishCards_kuoli.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '沃尔霍夫河',
    fishImage: '/path/to/fishCards_wolhe.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '北顿涅茨河',
    fishImage: '/path/to/fishCards_beidunnei.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '苏拉河',
    fishImage: '/path/to/fishCards_sula.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '拉多加湖',
    fishImage: '/path/to/fishCards_ladongjia.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '琥珀湖',
    fishImage: '/path/to/fishCards_hupo.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '拉多加湖群岛',
    fishImage: '/path/to/fishCards_ladongjiadao.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '阿赫图巴河',
    fishImage: '/path/to/fishCards_achtuba.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '钢湖',
    fishImage: '/path/to/fishCards_ganghu.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '下通古斯卡河',
    fishImage: '/path/to/fishCards_xiatongguska.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '亚马河',
    fishImage: '/path/to/fishCards_yama.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  },
  {
    name: '鄂威海',
    fishImage: '/path/to/fishCards_ewei.png', // 合并后的鱼类卡片图片路径
    fishes: [
      // 相应鱼类信息...
    ]
  }
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

main img {
  display: block;
  margin: auto;
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
