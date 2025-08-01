<script setup lang="ts">
import GrainyBackground from './components/GrainyBackground.vue'
import Container from './components/Container.vue'
import FlippingCard from './components/FlippingCard.vue'
import valentineCards from './data/valentineCard.json'
</script>

<template>
  <GrainyBackground class="h-screen p-12">
    <Container variant="convex" class="h-full w-full flex flex-col items-center justify-center">
      <div class="flex flex-col items-center justify-center h-full w-full">
        <h1 class="text-4xl font-bold text-white mb-8 drop-shadow-lg">💕 Valentine's Day Cards 💕</h1>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 max-w-7xl mx-auto">
          <FlippingCard 
            v-for="(card, index) in valentineCards" 
            :key="index"
            :cardData="card"
            class="mb-4"
          />
        </div>
        <p class="text-white/80 text-lg mt-8 text-center">Click on any card to see the romantic message! 💌</p>
      </div>
    </Container>
  </GrainyBackground>
</template>


