<template>
  <div class="app">
    <!-- 顶部标题 -->
    <header class="app-header">
      <div class="title">咖啡厅</div>
      <div class="subtitle">订单</div>
    </header>

    <!-- 主体内容 -->
    <div class="app-main">
      <!-- 左侧菜单 -->
      <aside class="sidebar">
        <div class="sidebar-inner">
          <div class="sidebar-title">水塘</div>
          <ul class="sidebar-menu">
            <li
              v-for="item in menus"
              :key="item.name"
              class="sidebar-item"
              :class="{ active: item.image === currentImage }"
              @click="handleMenuClick(item.image)"
            >
              <span class="arrow" :class="{ visible: item.image === currentImage }">▶</span>
              <span class="menu-text">{{ item.name }}</span>
            </li>
          </ul>
        </div>
      </aside>

      <!-- 右侧背景图 -->
      <main class="content">
        <!-- 圆形 Loading 动画 -->
        <div v-if="loading" class="loading-spinner"></div>

        <!-- 图片容器 -->
        <img
          v-show="!loading"
          :src="getImageUrl(currentImage)"
          alt="背景图"
          class="bg-image"
          @load="onImageLoad"
        />
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

// 自动生成图片映射表
const imageMap = Object.fromEntries(
  [
    '1.png',
    '2.png',
    '3.png',
    '4.png',
    '5.png',
    '6.png',
    '7.png',
    '8.png',
    '9.png',
    '10.png',
    '11.png',
    '12.png',
    '13.png',
    '14.png',
    '15.png',
    '16.png'
  ].map(name => [name, new URL(`./assets/cafe/${name}`, import.meta.url).href])
)

function getImageUrl(name: string): string {
  // 添加时间戳避免缓存
  return imageMap[name] ? `${imageMap[name]}?t=${Date.now()}` : ''
}

const menus = [
  { name: '克马羚诺也湖', image: '1.png' },
  { name: '惟有诺克河', image: '2.png' },
  { name: '旧奥斯特罗格湖', image: '3.png' },
  { name: '白河', image: '4.png' },
  { name: '廓里湖', image: '5.png' },
  { name: '沃尔霍夫河', image: '6.png' },
  { name: '北顿涅茨河', image: '7.png' },
  { name: '苏拉河', image: '8.png' },
  { name: '拉多加湖', image: '9.png' },
  { name: '琥珀湖', image: '10.png' },
  { name: '拉多加湖群岛', image: '11.png' },
  { name: '阿赫图巴河', image: '12.png' },
  { name: '铜湖', image: '13.png' },
  { name: '下通古斯卡河', image: '14.png' },
  { name: '亚马河', image: '15.png' },
  { name: '挪威海', image: '16.png' }
]

const currentImage = ref<string>(menus[0].image)
const loading = ref<boolean>(true) // 默认为 true，首次加载显示 loading

// 点击左侧菜单时触发
function handleMenuClick(imageName: string) {
  currentImage.value = imageName
  loading.value = true
}

// 图片加载完成后触发
function onImageLoad() {
  loading.value = false
}

// 👇 每分钟刷新一次图片
function startAutoRefresh() {
  setInterval(() => {
    const prevImage = currentImage.value
    loading.value = true

    // 强制 Vue 更新 img src，即使图片名不变也刷新
    currentImage.value = '' as any // 清空一下保证下次赋值会触发更新
    setTimeout(() => {
      currentImage.value = prevImage
    }, 0)
  }, 60 * 1000) // 每 60 秒刷新一次
}

// 页面加载完成后启动定时器
onMounted(() => {
  loading.value = false
  startAutoRefresh()
})
</script>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background-color: #000;
  color: white;
  font-family: sans-serif;
}

.app-header {
  background: #ccc;
  color: #000;
  text-align: center;
  padding: 12px 0 6px;
}

.title {
  font-size: 20px;
  font-weight: bold;
}

.subtitle {
  font-size: 14px;
  margin-top: 4px;
}

.app-main {
  display: flex;
  flex: 1;
  overflow: hidden;
}

.sidebar {
  width: 220px;
  background-color: rgba(0, 0, 0, 0.95);
  padding: 25px 10px;
  overflow-y: auto;
  margin-left: 80px;
}

.sidebar-inner {
  height: calc(100% - 50px);
  border-right: 1px solid rgba(255, 255, 255, 0.1);
}

.sidebar-title {
  font-weight: bold;
  font-size: 16px;
  margin-bottom: 16px;
  border-bottom: 2px solid #999999;
  line-height: 40px;
}

.sidebar-menu {
  list-style: none;
  padding: 0;
  margin: 0;
}

.sidebar-item {
  display: flex;
  align-items: center;
  cursor: pointer;
  font-size: 16px;
  color: #999;
  padding-left: 10px;
  line-height: 24px;
}

.sidebar-item:hover {
  color: inherit;
}

.sidebar-item.active .menu-text {
  color: #fff;
}

.arrow {
  width: 16px;
  display: inline-block;
  color: #ffff00;
  opacity: 0;
  transition: opacity 0.2s ease;
}

.arrow.visible {
  opacity: 1;
}

.menu-text {
  margin-left: 4px;
}

.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: black;
  position: relative;
}

.bg-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

/* 圆形 Loading 样式 */
.loading-spinner {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 40px;
  height: 40px;
  border: 4px solid rgba(255, 255, 255, 0.2);
  border-left-color: #ffffff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}
</style>
