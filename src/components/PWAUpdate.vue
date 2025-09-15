<script setup>
import { ref } from 'vue'
import { registerSW } from 'virtual:pwa-register'

// 创建更新标志
const needRefresh = ref(false)
const offlineReady = ref(false)

// 添加方法用于手动触发更新检查和离线就绪（仅用于演示）
const triggerUpdate = () => {
  needRefresh.value = true
}

const triggerOfflineReady = () => {
  offlineReady.value = true
}

// 暴露方法给父组件
defineExpose({
  triggerUpdate,
  triggerOfflineReady
})

// 注册Service Worker并获取更新函数
const updateSW = registerSW({
  onNeedRefresh() {
    // 当有新版本可用时触发
    needRefresh.value = true
  },
  onOfflineReady() {
    // 当应用可以离线使用时触发
    offlineReady.value = true
  },
})

// 更新应用
const updateServiceWorker = () => {
  // 调用updateSW()函数来执行更新
  updateSW()
  needRefresh.value = false
}

// 关闭提示
const close = () => {
  needRefresh.value = false
  offlineReady.value = false
}
</script>

<template>
  <div v-if="needRefresh || offlineReady" class="pwa-toast">
    <div class="message">
      <span v-if="needRefresh">
        有新版本可用，点击刷新按钮更新应用。
      </span>

      <span v-else-if="offlineReady">
        应用已准备好离线使用。
      </span>
    </div>

    <div class="buttons">
      <button v-if="needRefresh" @click="updateServiceWorker">刷新</button>
      <button @click="close">关闭</button>
    </div>
  </div>
</template>

<style scoped>
.pwa-toast {
  position: fixed;
  right: 0;
  bottom: 0;
  margin: 16px;
  padding: 12px;
  border: 1px solid #8885;
  border-radius: 4px;
  z-index: 100;
  background-color: white;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.message {
  font-size: 14px;
}

.buttons {
  display: flex;
  gap: 8px;
}

button {
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:first-child {
  background-color: #42b883;
  color: white;
}

button:last-child {
  background-color: #eee;
}
</style>
