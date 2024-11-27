<script setup>
import { ref, watch } from 'vue'
import BaseCard from './BaseCard.vue'

const props = defineProps({
  posts: {
    type: Array,
    required: true
  },
  lastLikedPostId: {
    type: Number,
    default: null
  },
  autoScroll: {
    type: Boolean,
    default: true
  }
})

const emit = defineEmits(['like-clicked'])

const postRefs = ref([])

const scrollToPost = (postId) => {
  const index = props.posts.findIndex(p => p.id === postId)
  if (index !== -1 && postRefs.value[index]) {
    postRefs.value[index].$el.scrollIntoView({ 
      behavior: 'smooth', 
      block: 'center' 
    })
  }
}

// Lägg till watch för lastLikedPostId
watch(() => props.lastLikedPostId, (newId) => {
  if (newId && props.autoScroll) {
    scrollToPost(newId)
  }
})

const handleLike = (postId) => {
  emit('like-clicked', postId)
  if (props.autoScroll) {
    scrollToPost(postId)
  }
}

// Lägg till en computed property för att kolla om en post är senast gillad
const isLastLiked = (postId) => {
  return props.lastLikedPostId === postId
}
</script>

<template>
  <div class="posts-container">
    <h2>Posts</h2>
    <div class="post-list">
      <BaseCard
        v-for="(post, index) in posts"
        :key="post.id"
        :ref="el => postRefs[index] = el"
        :variant="isLastLiked(post.id) ? 'highlight' : 'default'"
        :class="{ 'last-liked': isLastLiked(post.id) }"
      >
        <template #header>
          <h3>{{ post.title }}</h3>
        </template>

        <p>{{ post.content }}</p>
        
        <template #footer>
          <div class="likes-section">
            <span class="likes-count">{{ post.likes }} likes</span>
            <button 
              @click="handleLike(post.id)" 
              class="like-button"
            >
              ❤️ Like
            </button>
          </div>
        </template>
      </BaseCard>
    </div>
  </div>
</template>

<style scoped>
.likes-section {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.like-button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background: #e0e0e0;
  transition: all 0.2s ease;
}

.like-button:hover {
  background: #d0d0d0;
}

.loading {
  text-align: center;
  padding: 2rem;
  color: #666;
}

.posts-container {
  display: flex;
  flex-direction: column;
}

.post-list {
  padding-right: 1rem;
  scroll-behavior: smooth;
}

.post-list::-webkit-scrollbar {
  width: 8px;
}

.post-list::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 4px;
}

.post-list::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 4px;
}

.post-list::-webkit-scrollbar-thumb:hover {
  background: #555;
}

.last-liked {
  animation: highlight 2s ease-out;
}

@keyframes highlight {
  0% {
    transform: scale(1);
    box-shadow: 0 0 0 rgba(66, 184, 131, 0);
  }
  10% {
    transform: scale(1.02);
    box-shadow: 0 0 20px rgba(66, 184, 131, 0.4);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 0 rgba(66, 184, 131, 0);
  }
}
</style>