<template>
  <div class="app">
    <!-- 顶部标题 -->
    <header class="app-header">咖啡厅</header>

    <!-- 主体内容 -->
    <div class="app-main">
      <!-- 左侧菜单 -->
      <aside class="sidebar">
        <div class="sidebar-title">水塘</div>
        <ul class="sidebar-menu">
          <li
            v-for="item in menus"
            :key="item.name"
            class="sidebar-item"
            :class="{ active: item.image === currentImage }"
            @click="currentImage = item.image"
          >
            ▶ {{ item.name }}
          </li>
        </ul>
      </aside>

      <!-- 右侧内容区域 -->
      <main class="content">
        <img
          :src="getImageUrl(currentImage)"
          alt="背景图"
          class="bg-image"
        />
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// 图片路径生成函数（兼容 Vite 打包）
function getImageUrl(name: string): string {
  return new URL(`./assets/${name}`, import.meta.url).href
}

const menus = [
  { name: '克马尔蒂诺比湖', image: 'bg1.jpg' },
  { name: '旧马尔蒂诺比湖', image: 'bg2.jpg' },
  { name: '白河', image: 'bg3.jpg' },
  { name: '腾空湖', image: 'bg4.jpg' },
  { name: '沃尔霍夫河', image: 'bg5.jpg' },
  { name: '北极圈叉河', image: 'bg6.jpg' },
  { name: '芬芬湖', image: 'bg7.jpg' },
  { name: '拉古拉湖', image: 'bg8.jpg' },
  { name: '拉克加娜群岛', image: 'bg9.jpg' },
  { name: '阿穆尔河口', image: 'bg10.jpg' },
  { name: '钓鱼港', image: 'bg11.jpg' },
  { name: '下游叶卡卡河', image: 'bg12.jpg' },
  { name: '亚马河', image: 'bg13.jpg' },
  { name: '娜威湾', image: 'bg14.jpg' }
]

const currentImage = ref<string>(menus[0].image)
</script>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background-color: #000;
}

.app-header {
  background: #ccc;
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  padding: 12px 0;
}

.app-main {
  display: flex;
  flex: 1;
  overflow: hidden;
}

.sidebar {
  width: 220px;
  background-color: rgba(0, 0, 0, 0.9);
  color: white;
  padding: 20px 10px;
  font-family: sans-serif;
  overflow-y: auto;
  border-right: 1px solid rgba(255, 255, 255, 0.1);
}

.sidebar-title {
  font-weight: bold;
  font-size: 16px;
  margin-bottom: 16px;
}

.sidebar-menu {
  list-style: none;
  padding: 0;
  margin: 0;
}

.sidebar-item {
  padding: 6px 0;
  cursor: pointer;
  font-size: 14px;
  transition: color 0.2s;
}

.sidebar-item:hover {
  color: #f0c040;
}

.sidebar-item.active {
  color: #f0c040;
}

.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: black;
}

.bg-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}
</style>
