<template>
  <section id="projects">
    <div ref="carousel" class="logo-carousel overflow-hidden whitespace-nowrap border-b border-muted py-20 lg:py-40">
      <div class="logo-track inline-flex">
        <img v-for="(logo, i) in logos" :key="i" :src="logo"
          class="h-6 lg:h-10 mx-4 lg:mx-8 opacity-80 hover:opacity-100 transition-opacity" />
        <img v-for="(logo, i) in logos" :key="'dup-' + i" :src="logo"
          class="h-6 lg:h-10 mx-4 lg:mx-8 opacity-80 hover:opacity-100 transition-opacity" />
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const logos = [
  '/svg/client01.svg',
  '/svg/client02.svg',
  '/svg/client03.svg',
  '/svg/client04.svg',
  '/svg/client05.svg',
  '/svg/client06.svg',
  '/svg/client07.svg',
  '/svg/client08.svg',
]
const carousel = ref<HTMLElement | null>(null)
let rafId: number
let pos = 0             // float accumulator
const speed = 0.5      // px/frame

const safeLogos = logos.filter(l => typeof l === 'string' && l.trim() !== '')

function step() {
  if (!carousel.value) return
  const el = carousel.value

  pos += speed
  const half = el.scrollWidth / 2

  // wrap around
  if (pos >= half) pos -= half

  // write integer scrollLeft but keep fractional in pos
  el.scrollLeft = pos

  rafId = requestAnimationFrame(step)
}

onMounted(() => {
  rafId = requestAnimationFrame(step)
})

</script>

<style scoped>
.logo-track>* {
  flex-shrink: 0;
}

.logo-carousel img {
  filter: none;
  transition: filter 0.2s;
}

.dark .logo-carousel img {
  filter: brightness(1);
}
</style>