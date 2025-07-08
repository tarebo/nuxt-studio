<script setup lang="ts">
// ホームページ用のコンテンツを取得
const { data: page } = await useAsyncData('home', () => {
  return queryCollection('content').path('/').first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

// ホームページ用のSEO設定
useSeoMeta({
  title: page.value.title,
  description: page.value.description,
})
</script>

<template>
  <div class="home-template">

    <!-- ホームページ専用のコンテンツエリア -->
    <main class="home-content">
      <ContentRenderer
        v-if="page"
        :value="page"
        class="home-renderer"
      />
    </main>

    <!-- ホームページ専用のフッター -->
    <footer class="home-footer">
      <nav class="home-nav">
        <NuxtLink to="/story" class="nav-link">
          Stories
        </NuxtLink>
      </nav>
    </footer>
  </div>
</template>

<style scoped>

.home-subtitle {
  font-size: 1.2rem;
  opacity: 0.9;
}

.home-content {
  flex: 1;
  padding: 3rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.home-renderer {
  line-height: 1.8;
}

.home-footer {
  background: #f8f9fa;
  padding: 2rem;
  text-align: center;
  border-top: 1px solid #e9ecef;
}

.home-nav {
  display: flex;
  justify-content: center;
  gap: 2rem;
}

.nav-link {
  padding: 0.75rem 1.5rem;
  /* background: #667eea; */
  text-decoration: underline;
  color: #667eea;
  transition: all 0.3s ease;
  font-weight: 500;
}

.nav-link:hover {
  transform: translateY(-2px);
}

@media (max-width: 768px) {
  .home-header {
    padding: 2rem 1rem;
  }
  
  .home-title {
    font-size: 2rem;
  }
  
  .home-content {
    padding: 2rem 1rem;
  }
}
</style>
