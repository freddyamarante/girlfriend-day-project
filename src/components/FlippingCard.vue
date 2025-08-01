<script setup lang="ts">
import type { HTMLAttributes } from 'vue'
import { Motion } from 'motion-v'

interface CardData {
  emoji: string
  title: string
  content: string
  footnote: string
  instruction?: string
}

interface Props {
  customClass?: HTMLAttributes['class']
  cardData: CardData
  canBeFlipped?: boolean
  hoverTempo?: number
}

const props = defineProps<Props>()
const isFlipped = defineModel<boolean>('isFlipped', { default: false })

const handleClick = () => {
  if (props.canBeFlipped) {
    isFlipped.value = !isFlipped.value
  }
}
</script>

<template>
  <div class="w-80 h-96 perspective-1000" :class="props.customClass">
    <Motion
      :initial="{ rotateY: 0, y: 0 }"
      :animate="{ y: [-6, 6, -6], rotateY: isFlipped ? -180 : 0 }"
      :whileHover="canBeFlipped && !isFlipped ? { rotateY: -15, scale: 1.05 } : {scale: 1.05}"
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
      :transition-while-hover="{ 
        duration: hoverTempo || 0.3,
        ease: 'easeInOut'
      }"
      class="relative w-full h-full preserve-3d cursor-pointer"
      @click="handleClick"
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
          <p v-if="props.cardData.instruction" class="text-purple-800 text-sm mt-2 drop-shadow-md">{{ props.cardData.instruction }}</p>
        </div>
      </div>

      <!-- Back of the card -->
      <div class="absolute w-full h-full backface-hidden rounded-3xl flex items-center justify-center shadow-2xl bg-gradient-to-br from-pink-200 via-pink-300 to-purple-400 rotate-y-180 overflow-hidden p-6">
        <!-- Decorative elements -->
        <div class="absolute top-2 left-2 text-white/40 text-lg select-none">✨</div>
        <div class="absolute top-3 right-3 text-white/40 text-sm select-none">💫</div>
        <div class="absolute bottom-3 left-3 text-white/40 text-sm select-none">🌟</div>
        <div class="absolute bottom-2 right-2 text-white/40 text-lg select-none">✨</div>
        
        <!-- Romantic message with scrollable content -->
        <div class="text-center text-purple-900 z-10 w-full h-full flex flex-col justify-start items-center overflow-y-auto overflow-x-hidden scrollbar-thin scrollbar-thumb-purple-300 scrollbar-track-transparent py-4">
          <Motion
            :initial="{ scale: 1, opacity: 1 }"
            :whileHover="{ scale: 1.05, opacity: 0.9 }"
            :transition="{ type: 'spring', stiffness: 400, damping: 25 }"
            class="text-3xl font-bold drop-shadow-lg mb-4 select-none flex-shrink-0"
          >
            {{ props.cardData.emoji }}
          </Motion>
          <p
            class="text-lg font-semibold drop-shadow-lg leading-relaxed text-purple-900 flex-grow px-2"
          >
            {{ props.cardData.content }}
        </p>
          <span class="text-purple-800 text-sm mt-4 drop-shadow-md flex-shrink-0">{{ props.cardData.footnote }}</span>
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

/* Custom scrollbar styles */
.scrollbar-thin::-webkit-scrollbar {
  width: 6px;
}

.scrollbar-thin::-webkit-scrollbar-track {
  background: transparent;
}

.scrollbar-thumb-purple-300::-webkit-scrollbar-thumb {
  background: rgba(196, 181, 253, 0.6);
  border-radius: 3px;
}

.scrollbar-thumb-purple-300::-webkit-scrollbar-thumb:hover {
  background: rgba(196, 181, 253, 0.8);
}

.scrollbar-track-transparent::-webkit-scrollbar-track {
  background: transparent;
}
</style> 