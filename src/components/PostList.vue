<script setup>

</script>

<template>
  <div class="posts-container">
    <h2>Posts</h2>
    <div class="post-list">
        // Basecard goes here
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