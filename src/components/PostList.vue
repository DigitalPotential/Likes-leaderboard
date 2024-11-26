<script setup>
import BaseCard from './BaseCard.vue'

defineProps({
  posts: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['like-clicked'])

</script>

<template>
  <div class="posts-container">
    <h2>Posts</h2>
    <div class="post-list">
        <BaseCard
          v-for="post in posts"
          :key="post.id"
          varient="default"
        >
          <template #header>
            <h3>{{ post.title }}</h3>
          </template>

          <p>{{ post.content }}</p>

          <template #footer>
            <div class="likes-section">
              <span class="likes-count">{{ post.likes }} likes</span>

              <button
                class="like-button"
                @click="emit('like-clicked', post.id)"
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
</style>