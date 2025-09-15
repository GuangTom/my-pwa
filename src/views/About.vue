<template>
  <div class="about">
    <h1>关于</h1>
    <div class="about-content">
      <p>My PWA 是一个现代化的渐进式 Web 应用程序，具有以下特点：</p>
      <ul>
        <li>使用 Vue 3 和 Vite 构建</li>
        <li>支持离线工作</li>
        <li>可安装到主屏幕</li>
        <li>自动更新</li>
        <li>响应式设计，适配各种设备</li>
      </ul>
      <p>这个应用程序展示了 PWA 的核心功能，可以作为 PWA 项目的起点。</p>
    </div>
  </div>
</template>

<style scoped>
.about {
  padding: 2rem;
  max-width: 800px;
  margin: 0 auto;
}

.about-content {
  margin-top: 2rem;
  text-align: left;
  line-height: 1.6;
}

ul {
  margin: 1rem 0;
  padding-left: 2rem;
}

li {
  margin-bottom: 0.5rem;
}
</style>
