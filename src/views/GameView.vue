<script setup>
import { useRoute, useRouter } from 'vue-router'
import useGame from '../stores/useGame'

const route = useRoute()
const router = useRouter()

const { data, loading, error } = useGame(route.params.id)

const clickBack = () => {
  const isValidRoute = router.options.routes.some(
    (route) => route.path === router.options.history.state.back
  )

  if (isValidRoute) {
    router.back()
  } else {
    router.push('/games')
  }
}
</script>

<template>
  <main class="app-container">
    <button class="back-button" @click="clickBack">Return</button>
    <div v-if="loading">Loading</div>
    <div v-else-if="error">{{ state.error }}</div>
    <div v-else class="grid-container">
      <div class="image-grid">
        <img alt="Game Cover" class="game-cover" :src="data?.thumbnail" height="225" width="400" />
        <img
          v-for="screenshot of data?.screenshots"
          :key="screenshot.id"
          class="game-screenshot"
          :src="screenshot.image"
          height="225"
          width="400"
        />
      </div>
      <div class="info-container">
        <img alt="Game Cover" class="game-cover" :src="data?.thumbnail" height="150" width="300" />
        <h1>{{ data?.title }}</h1>
        <p>{{ data?.short_description }}</p>
        <div class="details">
          <p>Genre: {{ data?.genre }}</p>
          <p>Platform: {{ data?.platform }}</p>
          <p>Publisher: {{ data?.publisher }}</p>
          <p>Developer: {{ data?.developer }}</p>
          <p>Release Date: {{ data?.release_date }}</p>
        </div>
        <a :href="data?.game_url" target="_blank" class="download-link">Download Now</a>
      </div>
      <div class="description">
        <!-- <p>Game Description:</p> -->
        <p>{{ data?.description }}</p>
      </div>
      <div class="system-requirements">
        <h2>System Requirements</h2>
        <div class="requirement-card">
          <h3>Minimum</h3>
          <ul>
            <li><span>OS:</span> {{ data?.minimum_system_requirements?.os }}</li>
            <li>
              <span>Processor:</span>
              {{ data?.minimum_system_requirements?.processor }}
            </li>
            <li><span>Memory:</span> {{ data?.minimum_system_requirements?.memory }}</li>
            <li><span>Graphics:</span> {{ data?.minimum_system_requirements?.graphics }}</li>
            <li><span>Storage:</span> {{ data?.minimum_system_requirements?.storage }}</li>
          </ul>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.back-button {
  padding: 10px 15px;
  background: var(--color-input);
  border: none;
  cursor: pointer;
  margin-bottom: 1rem;
  color: var(--color-text-dark);
  font-weight: bold;
  transition:
    background-color 0.3s ease,
    color 0.3s ease;
  border-radius: 5px;

  &:hover {
    background-color: var(--color-border-light);
    color: var(--color-text-light);
  }
}

.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
}

.image-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}

.game-cover,
.game-screenshot,
.game-cover-small {
  object-fit: cover;
  border-radius: 0.5rem;
}

.info-container {
  margin-top: 2rem;
}

.info-container h1 {
  font-size: 2rem;
  font-weight: bold;
  color: var(--color-heading);
}

.info-container p {
  color: var(--color-text-dark);
  margin-top: 0.5rem;
}
.info-container img {
  width: 100%;
  height: auto;
  object-fit: cover;
  border-radius: 0.5rem;
}

.details {
  margin-top: 1rem;
}

.details p {
  color: var(--color-text-dark);
  margin-bottom: 0.5rem;
}

.download-link {
  display: inline-flex;
  align-items: center;
  font-weight: 500;
  text-decoration: none;
  color: var(--red);
  transition: 200ms color;
}

.download-link:hover {
  color: var(--yellow);
}
.description {
  color: var(--color-text-dark);
  margin-top: 1rem;
  font-size: 1.125rem;
  text-align: justify;
}

.system-requirements {
  padding: 1rem;
  margin-top: 3rem;
  max-width: 48rem;
  width: 100%;
}

.requirement-card {
  /* background-color: #faf9f6; */
  border-radius: 0.5rem;
  padding: 1rem;
  color: var(--color-text-dark);
}

.requirement-card h3 {
  font-size: 1.25rem;
  font-weight: bold;
}

.requirement-card ul {
  margin-top: 0.5rem;
}

.requirement-card ul li {
  margin-bottom: 0.5rem;
}

.requirement-card ul li span {
  font-weight: 500;
}

@media (max-width: 1024px) {
  .grid-container {
    grid-template-columns: 1fr;
  }

  .image-grid {
    grid-template-columns: 1fr;
  }

  .game-cover,
  .game-screenshot,
  .info-container img {
    display: grid;
    height: auto;
    width: 100%;
  }

  .info-container h1 {
    font-size: 1.5rem;
  }

  .description {
    font-size: 1rem;
  }
}

@media (min-width: 768px) and (max-width: 1024px) {
  .image-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 1rem;
  }
  .image-grid img {
    object-fit: cover;
    width: 100%;
    height: 100%;
  }
  .info-container img {
    width: 70%;
    height: auto;
  }
}

@media (max-width: 600px) {
  .info-container img {
    width: 80%;
    height: auto;
  }
}
</style>
