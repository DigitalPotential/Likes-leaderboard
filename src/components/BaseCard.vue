<script setup>
defineProps({
  variant: {
    type: String,
    default: 'default',
    validator: (value) => ['default', 'highlight'].includes(value)
  }
})
</script>

<template>
  <div class="base-card" :class="variant">
    <header class="card-header" v-if="$slots.header">
      <slot name="header"></slot>
    </header>
    
    <div class="card-content">
      <slot></slot>
    </div>
    
    <footer class="card-footer" v-if="$slots.footer">
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<style scoped>
.base-card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  padding: 1rem;
  margin-bottom: 1rem;
  transition: all 0.3s ease;
}

.base-card.highlight {
  border-left: 4px solid #42b883;
  background: #e8f5f0;
}

.card-header {
  margin-bottom: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 1px solid #eee;
}

.card-footer {
  margin-top: 1rem;
  padding-top: 0.5rem;
  border-top: 1px solid #eee;
}
</style> 