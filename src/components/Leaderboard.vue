<script setup>
import BaseCard from './BaseCard.vue';

defineProps({
  posts: {
    type: Array,
    required: true
  }
})

const getRankEmoji = (index) => {
  const emojis = ['🥇', '🥈', '🥉']
  return emojis[index] || `${index + 1}`
}

</script>

<template>
  <div class="leaderboard">
    <h2>🏆 Top Posts</h2>
        <BaseCard
          v-for="(post, index) in posts"
          :key="post.id"
          variant="highlight"
        >
          <template #header>
            <div class="rank-header">
              <span class="rank">{{ getRankEmoji(index) }}</span>
              <h4>{{ post.title }}</h4>
            </div>
          </template>

          <p>{{ post.summary }}</p>

          <template #footer>
            <span class="likes">{{ post.likes }} likes</span>
          </template>
        </BaseCard>
  </div>
</template>

<style scoped>
.leaderboard {
  background: #f8f9fa;
  padding: 1rem;
  border-radius: 8px;
}

.leaderboard h2 {
  margin-bottom: 1rem;
  color: #2c3e50;
}

.rank-header {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.rank {
  font-size: 1.5rem;
  min-width: 2rem;
}

.rank-header h4 {
  margin: 0;
  color: #2c3e50;
}

.likes {
  font-size: 0.875rem;
  color: #666;
}
</style>