<script setup lang="ts">
import GrainyBackground from './components/GrainyBackground.vue'
import Container from './components/Container.vue'
import FlippingCard from './components/FlippingCard.vue'
</script>

<template>
  <GrainyBackground class="h-screen p-12">
    <Container variant="convex" class="h-full w-full flex flex-col items-center justify-center">
      <div class="flex flex-col items-center justify-center h-full w-full">
        <FlippingCard class="mb-8" />
        <p class="text-white/80 text-lg">Hover over the card to see it flip!</p>
      </div>
    </Container>
  </GrainyBackground>
</template>


