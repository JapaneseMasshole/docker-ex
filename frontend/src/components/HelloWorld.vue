<script setup>
import { ref, onMounted } from 'vue'

defineProps({
  msg: {
    type: String,
    required: true,
  },
})

const backendMessage = ref('Loading...')
const error = ref(null)

onMounted(async () => {
  try {
    const response = await fetch('http://localhost:8000')
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    backendMessage.value = await response.text()
  } catch (error) {
    console.error('Error fetching from backend:', error)
    backendMessage.value = 'Error connecting to backend'
    error.value = error.message
  }
})
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      You've successfully created a project with
      <a href="https://vite.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
    </h3>
    <h4>
      Backend message:
      <span class="green">{{ backendMessage }}</span>
      <span v-if="error" class="error">Error: {{ error }}</span>
    </h4>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}

.error {
  color: red;
}
</style>
