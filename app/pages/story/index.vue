<script setup lang="ts">
// Storyページのリストを取得
const { data: stories } = await useAsyncData('stories', () => {
  return queryCollection('content').where('_path', { $regex: '/story' }).all()
})

// SEO設定
useSeoMeta({
  title: 'Stories - 雑司が谷のリリー',
  description: '珈琲店マルタのマスターの物語集',
})
</script>

<template>
  <div class="stories-template">
    <!-- ヘッダー -->
    <header class="stories-header">
      <NuxtLink to="/" class="back-link">
        ← Home
      </NuxtLink>
      <h1 class="stories-title">Stories</h1>
      <p class="stories-subtitle">珈琲店マルタのマスターの物語</p>
    </header>

    <!-- Story一覧 -->
    <main class="stories-content">
      <div class="stories-grid">
        <article
          v-for="story in stories"
          :key="story.path"
          class="story-card"
        >
          <NuxtLink :to="story.path" class="story-link">
            <h2 class="story-title">{{ story.title }}</h2>
            <p class="story-excerpt">{{ story.description || 'ストーリーを読む...' }}</p>
            <div class="story-meta">
              <time class="story-date">
                {{ new Date().toLocaleDateString('ja-JP') }}
              </time>
            </div>
          </NuxtLink>
        </article>
      </div>
    </main>

    <!-- フッター -->
    <footer class="stories-footer">
      <NuxtLink to="/" class="footer-home">
        Home へ戻る
      </NuxtLink>
    </footer>
  </div>
</template>

<style scoped>
.stories-template {
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}

.stories-header {
  background: white;
  padding: 2rem;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: relative;
}

.back-link {
  position: absolute;
  left: 2rem;
  top: 2rem;
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.back-link:hover {
  color: #764ba2;
}

.stories-title {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 0.5rem;
  font-weight: 700;
}

.stories-subtitle {
  color: #6c757d;
  font-size: 1.1rem;
}

.stories-content {
  padding: 3rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.stories-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.story-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.story-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.story-link {
  display: block;
  padding: 2rem;
  text-decoration: none;
  color: inherit;
}

.story-title {
  font-size: 1.5rem;
  color: #2c3e50;
  margin-bottom: 1rem;
  font-weight: 600;
}

.story-excerpt {
  color: #6c757d;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.story-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #e9ecef;
  padding-top: 1rem;
}

.story-date {
  color: #adb5bd;
  font-size: 0.9rem;
}

.stories-footer {
  text-align: center;
  padding: 2rem;
}

.footer-home {
  padding: 0.75rem 2rem;
  background: #667eea;
  color: white;
  text-decoration: none;
  border-radius: 25px;
  transition: all 0.3s ease;
  font-weight: 500;
  display: inline-block;
}

.footer-home:hover {
  background: #764ba2;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

@media (max-width: 768px) {
  .stories-header {
    padding: 1.5rem 1rem;
  }
  
  .back-link {
    left: 1rem;
    top: 1.5rem;
  }
  
  .stories-title {
    font-size: 2rem;
  }
  
  .stories-content {
    padding: 2rem 1rem;
  }
  
  .stories-grid {
    grid-template-columns: 1fr;
  }
  
  .story-link {
    padding: 1.5rem;
  }
}
</style>
