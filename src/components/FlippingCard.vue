<script setup lang="ts">
import type { HTMLAttributes } from 'vue'
import { Motion } from 'motion-v'
import { ref } from 'vue'

interface CardData {
  emoji: string
  title: string
  content: string
  footnote: string
  instruction: string
}

interface Props {
  class?: HTMLAttributes['class']
  cardData: CardData
}

const props = defineProps<Props>()
const isFlipped = ref(false)

const toggleFlip = () => {
  isFlipped.value = !isFlipped.value
}
</script>

<template>
  <div class="w-80 h-96 perspective-1000" :class="props.class">
    <Motion
      :initial="{ rotateY: 0, y: 0 }"
      :animate="{ y: [-6, 6, -6], rotateY: isFlipped ? 180 : 0 }"
      :transition="{ 
        type: 'spring', 
        stiffness: 300, 
        damping: 30,
        y: {
          duration: 4,
          repeat: Infinity,
          ease: 'easeInOut'
        }
      }"
      :whileHover="{
        scale: 1.05,
      }"
      class="relative w-full h-full preserve-3d cursor-pointer"
      @click="toggleFlip"
    >
      <!-- Front of the card -->
      <div class="absolute w-full h-full backface-hidden rounded-3xl flex items-center justify-center shadow-2xl bg-gradient-to-br from-pink-300 via-pink-400 to-purple-500 rotate-y-0 overflow-hidden">
        <!-- Decorative hearts -->
        <div class="absolute top-4 left-4 text-white/40 text-2xl select-none">💜</div>
        <div class="absolute top-6 right-6 text-white/40 text-xl select-none">💖</div>
        <div class="absolute bottom-6 left-6 text-white/40 text-xl select-none">💕</div>
        <div class="absolute bottom-4 right-4 text-white/40 text-2xl select-none">💝</div>
        
        <!-- Main content -->
        <div class="text-center text-purple-900 z-10">
          <Motion
            :initial="{ scale: 1, opacity: 1 }"
            :whileHover="{ scale: 1.05, opacity: 0.9 }"
            :transition="{ type: 'spring', stiffness: 400, damping: 25 }"
            class="text-6xl font-bold drop-shadow-lg mb-3 select-none"
          >
            {{ props.cardData.emoji }}
          </Motion>
          <Motion
            :initial="{ scale: 1, opacity: 1 }"
            :whileHover="{ scale: 1.05, opacity: 0.9 }"
            :transition="{ type: 'spring', stiffness: 400, damping: 25 }"
            class="text-2xl font-bold drop-shadow-lg text-purple-900"
          >
            {{ props.cardData.title }}
          </Motion>
          <p class="text-purple-800 text-sm mt-2 drop-shadow-md">{{ props.cardData.instruction }}</p>
        </div>
      </div>

      <!-- Back of the card -->
      <div class="absolute w-full h-full backface-hidden rounded-3xl flex items-center justify-center shadow-2xl bg-gradient-to-br from-pink-200 via-pink-300 to-purple-400 rotate-y-180 overflow-hidden p-6">
        <!-- Decorative elements -->
        <div class="absolute top-2 left-2 text-white/40 text-lg select-none">✨</div>
        <div class="absolute top-3 right-3 text-white/40 text-sm select-none">💫</div>
        <div class="absolute bottom-3 left-3 text-white/40 text-sm select-none">🌟</div>
        <div class="absolute bottom-2 right-2 text-white/40 text-lg select-none">✨</div>
        
        <!-- Romantic message -->
        <div class="text-center text-purple-900 z-10">
          <Motion
            :initial="{ scale: 1, opacity: 1 }"
            :whileHover="{ scale: 1.05, opacity: 0.9 }"
            :transition="{ type: 'spring', stiffness: 400, damping: 25 }"
            class="text-3xl font-bold drop-shadow-lg mb-4 select-none"
          >
            {{ props.cardData.emoji }}
          </Motion>
          <Motion
            :initial="{ scale: 1, opacity: 1 }"
            :whileHover="{ scale: 1.02, opacity: 0.9 }"
            :transition="{ type: 'spring', stiffness: 400, damping: 25 }"
            class="text-lg font-semibold drop-shadow-lg leading-relaxed text-purple-900"
          >
            {{ props.cardData.content }}
          </Motion>
          <p class="text-purple-800 text-sm mt-4 drop-shadow-md">{{ props.cardData.footnote }}</p>
        </div>
      </div>
    </Motion>
  </div>
</template>

<style scoped>
.perspective-1000 {
  perspective: 1000px;
}

.preserve-3d {
  transform-style: preserve-3d;
}

.backface-hidden {
  backface-visibility: hidden;
}

.rotate-y-0 {
  transform: rotateY(0deg);
}

.rotate-y-180 {
  transform: rotateY(180deg);
}
</style> 