<script setup>
import { ref, inject } from 'vue'
import { registerSW } from 'virtual:pwa-register'

const updateSWText = `const updateSW = registerSW({
  onNeedRefresh() { /* 有更新可用时的处理 */ },
  onOfflineReady() { /* 离线就绪时的处理 */ }
})`

const updateAvailable = ref(false)
const offlineReady = ref(false)
const updateStatus = ref('')

const triggerPWAUpdate = inject('triggerPWAUpdate')
const triggerPWAOfflineReady = inject('triggerPWAOfflineReady')

// 注册Service Worker并获取更新函数，调用它会触发Service Worker更新
const updateSW = registerSW({
  // 当有新版本可用时触发
  onNeedRefresh() {
    updateAvailable.value = true
    updateStatus.value = '发现新版本，可以更新应用'
  },
  // 当应用可以离线使用时触发
  onOfflineReady() {
    offlineReady.value = true
    updateStatus.value = '应用已准备好离线使用'
  },
  // 立即注册，默认为true
  immediate: true
})

// 手动检查更新
const checkForUpdates = () => {
  updateStatus.value = '正在检查更新...'
  // 模拟发现新版本
  setTimeout(() => {
    // 模拟触发onNeedRefresh回调
    updateAvailable.value = true
    updateStatus.value = '发现新版本，可以更新应用'
  }, 2000)
}

// 模拟离线就绪
const simulateOfflineReady = () => {
  updateStatus.value = '正在准备离线功能...'
  setTimeout(() => {
    // 模拟触发onOfflineReady回调
    offlineReady.value = true
    updateStatus.value = '应用已准备好离线使用'
    // 触发全局离线就绪提示
    triggerPWAOfflineReady()
  }, 2000)
}

// 应用更新
const applyUpdate = () => {
  // 调用updateSW函数执行更新
  // 这会重新加载页面并应用新版本
  updateSW()
}

// 跳过更新
const skipUpdate = () => {
  updateAvailable.value = false
  updateStatus.value = '已跳过更新'
}

// 重置
const reset = () => {
  updateAvailable.value = false
  offlineReady.value = false
  updateStatus.value = ''
}
</script>

<template>
  <div class="pwa-demo">
    <h1>演示</h1>

    <div class="status-card">
      <h2>Service Worker 状态</h2>
      <p>{{ updateStatus || '应用正常运行中' }}</p>

      <div class="status-indicators">
        <div class="indicator" :class="{ active: updateAvailable }">
          更新可用: {{ updateAvailable ? '是' : '否' }}
        </div>

        <div class="indicator" :class="{ active: offlineReady }">
          离线就绪: {{ offlineReady ? '是' : '否' }}
        </div>
      </div>
    </div>

    <div class="actions">
      <h3>Service Worker 操作</h3>

      <div class="buttons">
        <button
          @click="checkForUpdates"
          class="btn check"
        >
          检查更新
        </button>
        <button
          @click="applyUpdate"
          class="btn update"
          :disabled="!updateAvailable"
        >
          应用更新
        </button>
        <button
          @click="skipUpdate"
          class="btn skip"
          :disabled="!updateAvailable"
        >
          跳过更新
        </button>
        <button
          class="btn trigger"
          @click="triggerPWAUpdate"
        >
          模拟PWA更新提示
        </button>
        <button
          class="btn offline"
          @click="simulateOfflineReady"
        >
          模拟离线就绪
        </button>
        <button
          class="btn reset"
          @click="reset"
        >
          重置
        </button>
      </div>

      <div class="explanation">
        <h3>updateSW 函数使用说明</h3>
        <p>在 main.js 中，我们通过 <code>registerSW</code> 获取了 <code>updateSW</code> 函数：</p>
        <pre><code>{{ updateSWText }}</code></pre>

        <p>当需要应用更新时，只需调用此函数：</p>
        <pre><code>// 调用updateSW()函数来执行更新
updateSW()</code></pre>

        <p>这将重新加载页面并应用新版本的Service Worker。</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.pwa-demo {
  padding: 2rem;
  max-width: 800px;
  margin: 0 auto;
}

.status-card {
  background-color: #f5f5f5;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.status-indicators {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-top: 15px;
}

.indicator {
  padding: 8px 12px;
  border-radius: 4px;
  background-color: #e0e0e0;
  color: #666;
}

.indicator.active {
  background-color: #42b883;
  color: white;
}

.actions {
  background-color: #f5f5f5;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.buttons {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

.btn {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn.check {
  background-color: #2c3e50;
  color: white;
}

.btn.update {
  background-color: #42b883;
  color: white;
}

.btn.skip {
  background-color: #e0e0e0;
  color: #333;
}

.btn.trigger {
  background-color: #ff7700;
  color: white;
}

.btn.offline {
  background-color: #0077ff;
  color: white;
}

.btn.reset {
  background-color: #ccc;
  color: white;
}

.explanation {
  background-color: white;
  border-radius: 4px;
  padding: 15px;
  margin-top: 20px;
}

pre {
  background-color: #f0f0f0;
  padding: 10px;
  border-radius: 4px;
  overflow-x: auto;
}

code {
  font-family: monospace;
}
</style>
