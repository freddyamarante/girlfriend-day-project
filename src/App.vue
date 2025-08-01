<script setup lang="ts">
import GrainyBackground from './components/GrainyBackground.vue'
import Container from './components/Container.vue'
import Carousel from './components/Carousel.vue'
import valentineCards from './data/valentineCard.json'
</script>

<template>
  <GrainyBackground class="h-screen p-12">
    <Container variant="convex" class="h-full w-full flex flex-col items-center justify-center">
      <div class="flex flex-col items-center justify-center h-full w-full">
        <Carousel :cards="valentineCards" />
      </div>
    </Container>
  </GrainyBackground>
</template>


