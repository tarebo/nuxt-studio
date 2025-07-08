<script setup lang="ts">
const route = useRoute()

const { data: page } = await useAsyncData('page-' + route.path, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

// SEO設定
useSeoMeta({
  title: page.value.title,
  description: page.value.description,
})

// パスに基づいてページタイプを判定
const pageType = computed(() => {
  if (route.path.startsWith('/story')) {
    return 'story'
  }
  return 'default'
})
</script>

<template>
  <div class="page-template" :class="`page-${pageType}`">
    <!-- ナビゲーション -->
    <nav class="page-nav">
      <NuxtLink to="/" class="nav-home">
        ← Home
      </NuxtLink>
      <div class="nav-title">
        {{ page?.title }}
      </div>
    </nav>

    <!-- コンテンツエリア -->
    <main class="page-content">
      <article class="content-wrapper">
        <ContentRenderer
          v-if="page"
          :value="page"
          class="content-renderer"
        />
      </article>
    </main>

    <!-- フッター -->
    <footer class="page-footer">
      <div class="footer-content">
        <NuxtLink to="/" class="footer-link">
          Home
        </NuxtLink>
        <span class="footer-separator">|</span>
        <NuxtLink to="/story" class="footer-link">
          Stories
        </NuxtLink>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.page-template {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.page-nav {
  background: white;
  border-bottom: 1px solid #e9ecef;
  padding: 1rem 2rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: sticky;
  top: 0;
  z-index: 100;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.nav-home {
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.nav-home:hover {
  color: #764ba2;
}

.nav-title {
  font-weight: 600;
  color: #333;
  font-size: 1.1rem;
}

.page-content {
  flex: 1;
  padding: 3rem 2rem;
  max-width: 800px;
  margin: 0 auto;
  width: 100%;
}

/* Story専用のスタイル */
.page-story .content-wrapper {
  background: #fafafa;
  border-radius: 12px;
  padding: 2.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}

.page-story .content-renderer {
  line-height: 1.9;
  font-size: 1.1rem;
}

.page-story .content-renderer h1 {
  color: #2c3e50;
  border-bottom: 3px solid #667eea;
  padding-bottom: 0.5rem;
  margin-bottom: 2rem;
}

/* デフォルトページのスタイル */
.page-default .content-wrapper {
  background: white;
  border-radius: 8px;
  padding: 2rem;
}

.content-renderer {
  line-height: 1.7;
}

.page-footer {
  background: #f8f9fa;
  padding: 1.5rem 2rem;
  text-align: center;
  border-top: 1px solid #e9ecef;
}

.footer-content {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}

.footer-link {
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.footer-link:hover {
  color: #764ba2;
}

.footer-separator {
  color: #adb5bd;
}

@media (max-width: 768px) {
  .page-nav {
    padding: 1rem;
  }
  
  .nav-title {
    font-size: 1rem;
  }
  
  .page-content {
    padding: 2rem 1rem;
  }
  
  .page-story .content-wrapper,
  .page-default .content-wrapper {
    padding: 1.5rem;
  }
}
</style>
