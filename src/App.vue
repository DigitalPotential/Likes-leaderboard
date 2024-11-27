<script setup>
import { ref, computed, onMounted, onBeforeUnmount, watch } from 'vue'
import PostList from './components/PostList.vue'
import Leaderboard from './components/Leaderboard.vue'

const posts = ref([])
const isLoading = ref(true)
const showNotification = ref(false)
const notificationMessage = ref('')
const lastLikedPostId = ref(null)
const simulateOtherUsers = ref(true)
const autoScroll = ref(true)

// Simulera en API-laddning
onMounted(async () => {
  // Hämta sparade posts från localStorage
  const savedPosts = localStorage.getItem('posts')
  
  // Simulera API-anrop
  await new Promise(resolve => setTimeout(resolve, 1000))
  
  if (savedPosts) {
    posts.value = JSON.parse(savedPosts)
  } else {
    posts.value = [
      { id: 1, title: 'Beautiful Sunset', content: 'A beautiful sunset over the ocean, with colors painting the sky in vibrant oranges and purples. The waves gently crash against the shore as seabirds soar overhead.', summary: 'A beautiful suns...', likes: 5 },
      { id: 2, title: 'Morning Coffee', content: 'A freshly brewed cup of coffee on a sunny morning, with steam rising from the perfectly crafted latte art. The aroma fills the entire room.', summary: 'A cup of coff...', likes: 6 },
      { id: 3, title: 'Coding Time', content: 'A developer working on their laptop, surrounded by multiple monitors displaying colorful code. The mechanical keyboard clicks satisfyingly with each keystroke.', summary: 'A developer worki...', likes: 7 },
      { id: 4, title: 'Mountain Hiking', content: 'Reaching the summit after a challenging hike, the panoramic view of the valley below makes every step worth it. The crisp mountain air fills your lungs.', summary: 'Reaching the sum...', likes: 3 },
      { id: 5, title: 'Urban Photography', content: 'Capturing the essence of city life through a camera lens. The streets are alive with movement, lights, and the stories of countless passersby.', summary: 'Capturing the...', likes: 8 },
      { id: 6, title: 'Cooking Adventure', content: 'Experimenting with new recipes in the kitchen, combining unexpected flavors to create something unique. The kitchen is filled with aromatic spices.', summary: 'Experimenting...', likes: 4 },
      { id: 7, title: 'Garden Progress', content: 'Watching the garden come to life with spring flowers blooming everywhere. Bees buzz around the colorful petals while butterflies dance in the sunshine.', summary: 'Watching the...', likes: 9 },
      { id: 8, title: 'Music Session', content: 'Lost in the rhythm of a late-night music session, fingers dancing across piano keys as melodies flow naturally. The room resonates with harmony.', summary: 'Lost in the...', likes: 5 },
      { id: 9, title: 'Art Creation', content: 'Brush strokes on canvas telling a story of imagination and creativity. Paint splatters and color combinations bring the artwork to life.', summary: 'Brush strokes...', likes: 6 },
      { id: 10, title: 'Tech Setup', content: 'A perfectly organized desk setup with all the latest gadgets. Cable management is on point, and RGB lights create the perfect ambiance.', summary: 'A perfectly...', likes: 7 },
      { id: 11, title: 'Book Collection', content: 'Shelves filled with books of various genres, each spine telling its own story. The smell of old pages fills the cozy reading nook.', summary: 'Shelves filled...', likes: 4 },
      { id: 12, title: 'Travel Memories', content: 'Reflecting on adventures from around the world, each photograph capturing a moment of discovery and wonder. The collection of memories grows with each journey.', summary: 'Reflecting on...', likes: 8 }
    ]
  }
  
  isLoading.value = false

  // Starta simulering av andra användares likes
  if (simulateOtherUsers.value) {
    startRandomLikes()
  }
})

// Spara posts när komponenten tas bort
onBeforeUnmount(() => {
  localStorage.setItem('posts', JSON.stringify(posts.value))
})

const totalLikes = computed(() => {
  return posts.value.reduce((sum, post) => sum + post.likes, 0)
})

const sortedPosts = computed(() => {
  return [...posts.value].sort((a, b) => b.likes - a.likes)
})

const handleLike = (postId) => {
  posts.value.find(p => p.id === postId).likes++
  lastLikedPostId.value = postId
}

watch(totalLikes, (newValue, oldValue) => {
  if (oldValue !== undefined && newValue > oldValue) {
    notificationMessage.value = `Totala likes ökade till ${newValue}! 🎉`
    showNotification.value = true
    setTimeout(() => {
      showNotification.value = false
    }, 2000)
  }
})

watch(posts, (newPosts) => {
  localStorage.setItem('posts', JSON.stringify(newPosts))
}, { deep: true })

watch(lastLikedPostId, (newId) => {
  if (newId) {
    const post = posts.value.find(p => p.id === newId)
    if (post) {
      notificationMessage.value = `Senast gillade post: "${post.title}"`
      showNotification.value = true
      setTimeout(() => {
        showNotification.value = false
      }, 2000)
    }
  }
})

const startRandomLikes = () => {
  const interval = setInterval(() => {
    if (posts.value.length > 0) {
      const randomPost = posts.value[Math.floor(Math.random() * posts.value.length)]
      randomPost.likes++
      lastLikedPostId.value = randomPost.id
      notificationMessage.value = `Någon gillade "${randomPost.title}"! ❤️`
      showNotification.value = true
      setTimeout(() => {
        showNotification.value = false
      }, 2000)
    }
  }, 5000) // Var 5:e sekund

  // Cleanup på unmount
  onBeforeUnmount(() => {
    clearInterval(interval)
  })
}

const resetPosts = () => {
  localStorage.removeItem('posts')
  window.location.reload()
}
</script>

<template>
  <div class="container">
    <div v-if="showNotification" class="notification">
      {{ notificationMessage }}
    </div>
    <header class="header">
      <div>Total Post Likes: {{ totalLikes }}</div>
      <div class="header-buttons">
        <button 
          @click="simulateOtherUsers = !simulateOtherUsers" 
          class="simulate-toggle"
        >
          {{ simulateOtherUsers ? 'Stoppa' : 'Starta' }} simulering
        </button>
        <button 
          @click="autoScroll = !autoScroll" 
          class="scroll-toggle"
          :class="{ 'inactive': !autoScroll }"
        >
          {{ autoScroll ? 'Stäng av' : 'Aktivera' }} auto-scroll
        </button>
        <button 
          @click="resetPosts" 
          class="reset-button"
        >
          Återställ Posts
        </button>
      </div>
    </header>

    <div class="content-layout">
      <PostList
        :posts="posts"
        :lastLikedPostId="lastLikedPostId"
        :autoScroll="autoScroll"
        @like-clicked="handleLike"
      />

      <Leaderboard
        :posts="sortedPosts"
      />
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
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #2c3e50;
  color: white;
  padding: 1rem;
  margin-bottom: 1rem;
}

.notification {
  position: fixed;
  top: 20px;
  right: 20px;
  background: #42b883;
  color: white;
  padding: 1rem;
  border-radius: 4px;
  animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0);
  }
}

.simulate-toggle {
  background: #42b883;
  border: none;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

.simulate-toggle:hover {
  background: #3aa876;
}

.reset-button {
  background: #e74c3c;
  border: none;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

.reset-button:hover {
  background: #c0392b;
}

.header-buttons {
  display: flex;
  gap: 1rem;
}

.scroll-toggle {
  background: #3498db;
  border: none;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

.scroll-toggle:hover {
  background: #2980b9;
}

.scroll-toggle.inactive {
  background: #95a5a6;
}

.scroll-toggle.inactive:hover {
  background: #7f8c8d;
}
</style>