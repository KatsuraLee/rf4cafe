<template>
  <div class="app">
    <!-- 顶部标题 -->
    <header class="app-header">
      <div class="title">咖啡厅</div>
      <div class="header-bottom">
        <div class="subtitle">订单</div>
        <div class="update-time" v-if="currentUpdateTime">更新时间: {{ currentUpdateTime }}</div>
      </div>
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

// 背景图片列表
const bgImages = Array.from({ length: 18 }, (_, i) => `/bg/bg${i + 1}.jpg`)
const randomBgImage = ref(`url(${bgImages[Math.floor(Math.random() * bgImages.length)]})`)

// 随机更换背景图片
function updateRandomBackground() {
  const newIndex = Math.floor(Math.random() * bgImages.length)
  randomBgImage.value = `url(${bgImages[newIndex]})`
}

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
    '16.png',
    '17.png'
  ].map(name => [name, new URL(`./assets/cafe/${name}`, import.meta.url).href])
)

function getImageUrl(name: string): string {
  // 添加时间戳避免缓存
  return imageMap[name] ? `${imageMap[name]}?t=${Date.now()}` : ''
}

const menus = [
  { name: '克马羚诺也湖', image: '1.png' },
  { name: '埃尔克湖', image: '2.png' },
  { name: '惟有诺克河', image: "3.png" },
  { name: '旧奥斯特罗格湖', image: '4.png' },
  { name: '白河', image: '5.png' },
  { name: '廓里湖', image: '6.png' },
  { name: '沃尔霍夫河', image: '7.png' },
  { name: '北顿涅茨河', image: '8.png' },
  { name: '苏拉河', image: '9.png' },
  { name: '拉多加湖', image: '10.png' },
  { name: '琥珀湖', image: '11.png' },
  { name: '拉多加湖群岛', image: '12.png' },
  { name: '阿赫图巴河', image: '13.png' },
  { name: '铜湖', image: '14.png' },
  { name: '下通古斯卡河', image: '15.png' },
  { name: '亚马河', image: '16.png' },
  { name: '挪威海', image: '17.png' }
]

// 默认选择"挪威海"（索引为15）
const currentImage = ref<string>(menus[15].image)
const currentUpdateTime = ref<string>('')
const loading = ref<boolean>(true)

// 定义 Screenshot 接口
interface Screenshot {
  image_number: number;
  datetime: string;
}

// 获取 update_time.json 的 URL
const updateTimeUrl = new URL('./assets/time/update_time.json', import.meta.url).href

// 加载更新时间数据
async function loadUpdateTime() {
  try {
    const response = await fetch(updateTimeUrl)
    const data = await response.json()
    const currentImageNumber = parseInt(currentImage.value.replace('.png', ''))
    const screenshot = data.screenshots.find((s: Screenshot) => s.image_number === currentImageNumber)
    if (screenshot) {
      const date = new Date(screenshot.datetime)
      currentUpdateTime.value = `${String(date.getHours()).padStart(2, '0')}:${String(date.getMinutes()).padStart(2, '0')}`
    }
  } catch (error) {
    console.error('Failed to load update time:', error)
  }
} // 默认为 true，首次加载显示 loading

// 点击左侧菜单时触发
function handleMenuClick(imageName: string) {
  currentImage.value = imageName
  loading.value = true
  loadUpdateTime() // 加载对应的更新时间
}

// 图片加载完成后触发
function onImageLoad() {
  loading.value = false
}

// 👇 每分钟刷新一次图片和更新时间
function startAutoRefresh() {
  setInterval(() => {
    const prevImage = currentImage.value
    loading.value = true

    // 强制 Vue 更新 img src，即使图片名不变也刷新
    currentImage.value = '' as any // 清空一下保证下次赋值会触发更新
    setTimeout(() => {
      currentImage.value = prevImage
      loadUpdateTime() // 更新时间显示
    }, 0)
  }, 60 * 1000) // 每 60 秒刷新一次
}

// 确保选中的菜单项在视图中可见
function scrollToActiveMenuItem() {
  setTimeout(() => {
    const activeItem = document.querySelector('.sidebar-item.active')
    if (activeItem) {
      activeItem.scrollIntoView({ behavior: 'smooth', block: 'center' })
    }
  }, 100)
}

// 页面加载完成后启动定时器
onMounted(() => {
  loading.value = false
  loadUpdateTime() // 加载初始更新时间
  startAutoRefresh()
  // 立即设置一个随机背景
  updateRandomBackground()
  // 每5分钟更换一次背景图片
  setInterval(updateRandomBackground, 5 * 60 * 1000)
  // 确保"挪威海"菜单项在视图中可见
  scrollToActiveMenuItem()
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
  background: #999;
  color: #000;
  text-align: center;
  padding: 12px 0 6px;
}

.header-bottom {
  position: relative;
  text-align: center;
  padding: 20px 0 8px;
}

.title {
  font-size: 26px;
  font-weight: bold;
}

.subtitle {
  font-size: 16px;
  color: #FFF;
  position: relative;
  display: inline-block;
  padding-bottom: 8px;
}

.update-time {
  position: absolute;
  left: 50%;
  margin-left: 40px; /* 距离订单文字10px */
  top: 60%;
  transform: translateY(-50%);
  font-size: 14px;
  color: #fff;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 4px 8px;
  border-radius: 4px;
}

.subtitle::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 120%; /* 比文字宽一点 */
  height: 4px;
  background-color: #b7c639; /* 绿色 */
  border-radius: 2px;
}

.app-main {
  display: flex;
  flex: 1;
  overflow: hidden;
  background-image: v-bind(randomBgImage);
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.sidebar {
  width: 300px;
  background-color: rgba(0, 0, 0, 0.7);
  padding: 25px 20px 0 80px;
  overflow-y: auto;
}

.sidebar-inner {
  padding: 20px;
  height: calc(100% - 50px);
  border-right: 1px solid rgba(255, 255, 255, 0.1);
}

.sidebar-title {
  font-weight: bold;
  font-size: 22px;
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
  font-size: 17px;
  color: #999;
  padding-left: 10px;
  line-height: 26px;
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
  background-color: rgba(0, 0, 0, 0.7);
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
