<script setup>
import { ref, computed } from 'vue'

const posts = ref([
  { id: 1, title: 'Beautiful Sunset', content: 'A beautiful sunset over the ocean.', summary: 'A beautiful suns...', likes: 5 },
  { id: 2, title: 'Morning Coffee', content: 'A cup of coffee on a sunny morning.', summary: 'A cup of coff...', likes: 6 },
  { id: 3, title: 'Coding Time', content: 'A developer working on their laptop.', summary: 'A developer worki...', likes: 7 }
])

const totalLikes = computed(() => {
  return posts.value.reduce((sum, post) => sum + post.likes, 0)
})

const sortedPosts = computed(() => {
  return [...posts.value].sort((a, b) => b.likes - a.likes)
})

const handleLike = (postId) => {
  posts.value.find(p => p.id === postId).likes++
}
</script>

<template>
  <div class="container">
    <header class="header">

    </header>

    <div class="content-layout">
      
    </div>
  </div>
</template>

<style scoped>
.content-layout {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 2rem;
  padding: 1rem;
}

.header {
  background: #2c3e50;
  color: white;
  padding: 1rem;
  text-align: center;
  font-size: 1.5rem;
  margin-bottom: 1rem;
}
</style>