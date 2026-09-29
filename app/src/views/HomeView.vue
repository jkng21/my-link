<script setup lang="ts">
import { ref, watch } from 'vue'

const isLight = ref(localStorage.getItem('jennifer-theme') === 'light')
const imageFailed = ref(false)

const links = [
  {
    label: 'Portfolio',
    description: 'Selected work and experiments',
    href: 'https://example.com/portfolio',
    external: true,
  },
  {
    label: 'Substack',
    description: 'Notes, ideas, and things I am learning',
    href: 'https://yourname.substack.com',
    external: true,
  },
  {
    label: 'LinkedIn',
    description: 'Professional updates and connections',
    href: 'https://www.linkedin.com/in/your-profile/',
    external: true,
  },
  {
    label: 'Email',
    description: 'A good place to say hello',
    href: 'mailto:hello@example.com',
    external: false,
  },
]

watch(
  isLight,
  (light) => {
    document.documentElement.dataset.theme = light ? 'light' : 'dark'
    localStorage.setItem('jennifer-theme', light ? 'light' : 'dark')
  },
  { immediate: true },
)
</script>

<template>
  <main class="page-shell">
    <section class="profile-card" aria-labelledby="profile-name">
      <div class="card-topline">
        <span class="eyebrow">PERSONAL LINKS</span>
        <button
          class="theme-toggle"
          type="button"
          :aria-label="`Switch to ${isLight ? 'dark' : 'light'} mode`"
          :aria-pressed="isLight"
          @click="isLight = !isLight"
        >
          <span class="toggle-track" aria-hidden="true"><span class="toggle-thumb"></span></span>
          <span>{{ isLight ? 'Light' : 'Dark' }}</span>
        </button>
      </div>

      <div class="profile-intro">
        <div class="avatar-frame">
          <img
            v-if="!imageFailed"
            class="avatar"
            src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=400&h=400&q=85"
            alt="Temporary profile photo placeholder"
            @error="imageFailed = true"
          />
          <span v-else class="avatar-fallback" aria-label="Profile photo placeholder">JK</span>
        </div>
        <h1 id="profile-name">Jennifer Kang</h1>
        <p class="tagline">A little corner of the internet for my work, words, and ways to say hello.</p>
      </div>

      <nav class="link-list" aria-label="Jennifer's links">
        <a
          v-for="(link, index) in links"
          :key="link.label"
          class="profile-link"
          :href="link.href"
          :target="link.external ? '_blank' : undefined"
          :rel="link.external ? 'noopener noreferrer' : undefined"
        >
          <span class="link-index">0{{ index + 1 }}</span>
          <span class="link-copy">
            <span class="link-label">{{ link.label }}</span>
            <span class="link-description">{{ link.description }}</span>
          </span>
          <span class="link-arrow" aria-hidden="true"></span>
        </a>
      </nav>

      <p class="card-footer">Thanks for stopping by.</p>
    </section>
  </main>
</template>