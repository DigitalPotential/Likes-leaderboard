<script setup>
import { computed } from 'vue'

const getRankEmoji = (index) => {
  const emojis = ['🥇', '🥈', '🥉']
  return emojis[index] || `${index + 1}`
}

</script>

<template>
  <div class="leaderboard">
    <h2>🏆 Top Posts</h2>
        // Basecard goes here
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