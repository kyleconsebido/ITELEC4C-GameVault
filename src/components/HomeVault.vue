<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { IconBrowser, IconWindows } from '../assets/icons'
import useIsIntersecting from '../composables/useIsIntersecting'
import useGames from '../stores/useGames'
import scrollTop from '../utils/scrollTop'

const element = ref(null)

const isIntersecting = useIsIntersecting(element, { threshold: 0.5 })

const router = useRouter()

const { filters } = useGames()

const redirectToPlatform = (platform) => {
  filters.value = [{ field: 'platform', value: platform }]
  router.push('/games')
  scrollTop()
}
</script>

<template>
  <section
    :ref="(el) => (element = el)"
    :class="{ hide: !isIntersecting }"
    class="app-container vault"
  >
    <img src="@/assets/images/vault-clipart.svg" />
    <div class="vault-info">
      <span>
        <h1 class="section-title">
          <span class="vault-number">300+</span>
          <br />Games
        </h1>
        <div class="platforms">
          <button class="platform-btn windows" @click="redirectToPlatform('PC (Windows)')">
            <IconWindows class="platform-icon" />
          </button>
          <button class="platform-btn browser" @click="redirectToPlatform('Web Browser')">
            <IconBrowser class="platform-icon" />
          </button>
        </div>
      </span>
    </div>
  </section>
</template>

<style scoped>
.vault {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  justify-content: center;
  transition: 500ms opacity;
}

.vault img {
  width: 100%;
  opacity: 1;
  transition:
    500ms opacity 200ms,
    500ms translate 200ms;

  @media (max-width: 768px) {
    display: none;
  }
}

.vault.hide img {
  opacity: 0;
  translate: 0 1em;
}

.vault-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20%;

  transition:
    500ms opacity 300ms,
    500ms translate 300ms;
}

.vault.hide .vault-info {
  opacity: 0;
  translate: -2em 0;
}

.vault-number {
  background: var(--color-theme-gradient-y);
  background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: inherit;
}

.platforms {
  display: flex;
  column-gap: 1rem;
  margin-top: 1rem;
}

.platform-btn {
  --color-background: var(--color-background);

  position: relative;
  font-size: 1rem;
  width: 3.5em;
  height: 3.5em;
  padding: 1em;
  align-items: center;
  border: 1px solid var(--color-border-light);
  border-radius: var(--border-radius);
  background-color: transparent;
  color: var(--color-text-light);
  outline: none;
  cursor: pointer;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  backface-visibility: none;
  transition: scale 200ms;
}

.platform-btn::before {
  content: '';
  z-index: 0;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border-radius: inherit;
  background: radial-gradient(
      circle at top right,
      color-mix(in srgb, var(--color-background) 100%, transparent) -50%,
      transparent 100%
    )
    color-mix(in srgb, var(--black-1) 50%, transparent);
  opacity: 0.8;
  scale: 1.1;
  transition: 200ms opacity;
}

.platform-btn.windows {
  --color-background: #087cd5;
}

.platform-btn.browser {
  --color-background: var(--red);
}

.platform-btn:hover {
  scale: 1.05;
}

.platform-btn:hover::before {
  opacity: 1;
}

.platform-icon {
  position: relative;
  justify-self: right;
  width: 100%;
  height: 100%;
  z-index: 2;
}

.windows .platform-icon {
  fill: white;
}
</style>
