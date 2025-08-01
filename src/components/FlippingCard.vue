<script setup lang="ts">
import type { HTMLAttributes } from 'vue'
import { Motion } from 'motion-v'
import { ref } from 'vue'

interface Props {
  class?: HTMLAttributes['class']
}

const props = defineProps<Props>()
const isFlipped = ref(false)

const toggleFlip = () => {
  isFlipped.value = !isFlipped.value
}
</script>

<template>
  <div class="w-52 h-52 perspective-1000" :class="props.class">
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
        scale: 1.2,
      }"
      class="relative w-full h-full preserve-3d cursor-pointer"
      @click="toggleFlip"
    >
      <div class="absolute w-full h-full backface-hidden rounded-2xl flex items-center justify-center shadow-2xl bg-gradient-to-br from-indigo-500 to-purple-600 rotate-y-0">
        <Motion
          :initial="{ scale: 1, opacity: 1 }"
          :whileHover="{ scale: 1.05, opacity: 0.9 }"
          :transition="{ type: 'spring', stiffness: 400, damping: 25 }"
          class="text-center text-white text-3xl font-bold drop-shadow-lg"
        >
          Hello
        </Motion>
      </div>
      <div class="absolute w-full h-full backface-hidden rounded-2xl flex items-center justify-center shadow-2xl bg-gradient-to-br from-pink-400 to-red-500 rotate-y-180">
        <Motion
          :initial="{ scale: 1, opacity: 1 }"
          :whileHover="{ scale: 1.05, opacity: 0.9 }"
          :transition="{ type: 'spring', stiffness: 400, damping: 25 }"
          class="text-center text-white text-3xl font-bold drop-shadow-lg"
        >
          World
        </Motion>
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