<script setup lang="ts">
import GrainyBackground from './components/GrainyBackground.vue'
import Container from './components/Container.vue'
import Carousel from './components/Carousel.vue'
import valentineCards from './data/valentineCard.json'
</script>

<template>
  <GrainyBackground customClass="h-screen p-12">
    <Container variant="convex" class="h-full w-full flex flex-col items-center justify-center">
      <h1 class="text-4xl font-bold text-white drop-shadow-lg mb-8">💕 Cartas para Patima - Feliz Día de la Novia 💕</h1>
      <h2 class="text-white/80 text-lg drop-shadow-lg mb-8">Haz clic en una carta para ver el mensaje</h2>
        <Carousel :cards="valentineCards" customClass="z-50 relative" />
    </Container>
    <span class="absolute right-12 bottom-4 text-white/80 text-lg drop-shadow-lg">
      Hecho por: <a href="https://github.com/freddyamarante" target="_blank" class="text-white underline">Ate (Github)</a>
    </span>
  </GrainyBackground>
</template>


