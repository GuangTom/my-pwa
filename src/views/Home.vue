<script setup>
import { ref } from 'vue'

const count = ref(0)
const isOnline = ref(navigator.onLine)

// 监听在线状态变化
window.addEventListener('online', () => isOnline.value = true)
window.addEventListener('offline', () => isOnline.value = false)
</script>

<template>
  <div class="home">
    <h1>主页</h1>

    <div class="pwa-status">
      <div :class="['status-indicator', { 'online': isOnline, 'offline': !isOnline }]">
        {{ isOnline ? '在线' : '离线' }}
      </div>
    </div>

    <div class="card">
      <button type="button" @click="count++">计数器: {{ count }}</button>

      <p>这是一个支持 PWA 的 Vue 应用</p>
    </div>

    <div class="pwa-features">
      <h2>PWA 功能</h2>
      <ul>
        <li>✅ 离线工作</li>
        <li>✅ 可安装到主屏幕</li>
        <li>✅ 自动更新</li>
        <li>✅ 推送通知支持</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.home {
  padding: 2rem;
}

.pwa-status {
  margin: 1rem 0;
}

.status-indicator {
  display: inline-block;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  font-weight: bold;
}

.online {
  background-color: #4caf50;
  color: white;
}

.offline {
  background-color: #f44336;
  color: white;
}

.pwa-features {
  margin: 1rem 0;
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.pwa-features h2 {
  margin-top: 0;
  color: #2c3e50;
}

.pwa-features ul {
  text-align: left;
  padding-left: 1.5rem;
}
</style>
