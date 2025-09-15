<script setup>
// 导入路由和PWA更新组件
import PWAUpdate from './components/PWAUpdate.vue'
import { ref, provide } from 'vue'

// 创建PWAUpdate组件的引用
const pwaUpdateRef = ref(null)

// 提供方法来触发PWA更新检查和离线就绪（仅用于演示）
const triggerPWAUpdate = () => {
  if (pwaUpdateRef.value) {
    pwaUpdateRef.value.triggerUpdate()
  }
}

const triggerPWAOfflineReady = () => {
  if (pwaUpdateRef.value) {
    pwaUpdateRef.value.triggerOfflineReady()
  }
}

// 通过provide提供给子组件使用
provide('triggerPWAUpdate', triggerPWAUpdate)
provide('triggerPWAOfflineReady', triggerPWAOfflineReady)
</script>

<template>
  <div class="app">
    <header>
      <div class="logo-container">
        <img src="/vite.svg" class="logo" alt="Vite logo" />
        <h1>My PWA</h1>
      </div>

      <nav>
        <router-link to="/">主页</router-link>
        <router-link to="/about">关于</router-link>
        <router-link to="/demo">演示</router-link>
      </nav>
    </header>

    <main class="main-content">
      <router-view />
    </main>

    <footer>
      <p>2025 My PWA - 基于 Vue3 和 Vite 的 PWA 应用</p>
    </footer>

    <!-- PWA 更新提示组件 -->
    <PWAUpdate ref="pwaUpdateRef" />
  </div>
</template>

<style>
.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

header {
  background-color: #2c3e50;
  color: white;
  padding: 1rem;
}

.logo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}

.logo {
  height: 3em;
}

.logo:hover {
  filter: drop-shadow(0 0 1em #646cffaa);
}

nav {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

nav a {
  color: white;
  text-decoration: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  transition: background-color 0.3s;
}

nav a:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

nav a.router-link-active {
  background-color: rgba(255, 255, 255, 0.2);
  font-weight: bold;
}

main {
  flex: 1;
  padding: 1rem;
  width: 75vw;
}

footer {
  background-color: #f9f9f9;
  padding: 1rem;
  text-align: center;
  color: #666;
  border-top: 1px solid #eee;
}
</style>
