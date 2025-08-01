<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import FlippingCard from './FlippingCard.vue'

interface CardData {
  emoji: string
  title: string
  content: string
  footnote: string
  instruction?: string
}

interface Props {
  cards: CardData[]
  class?: string
}

const props = defineProps<Props>()

const currentIndex = ref(0)
const cardStates = ref<boolean[]>(new Array(props.cards.length).fill(false))

// Computed properties for navigation state
const isFirstCard = computed(() => currentIndex.value === 0)
const isLastCard = computed(() => currentIndex.value === props.cards.length - 1)

// Navigation functions
const nextCard = () => {
  if (!isLastCard.value) {
    currentIndex.value++
  }
}

const prevCard = () => {
  if (!isFirstCard.value) {
    currentIndex.value--
  }
}

const goToCard = (index: number) => {
  if (index >= 0 && index < props.cards.length) {
    currentIndex.value = index
  }
}

// Handle card flip state changes
const handleCardStateChange = (index: number, value: boolean) => {
  if (value) {
    cardStates.value.forEach((_, i) => {
      if (i !== index) {
        cardStates.value[i] = false
      }
    })
  }
}

// Watch for card state changes
cardStates.value.forEach((_, index) => {
  watch(() => cardStates.value[index], (newValue) => {
    handleCardStateChange(index, newValue)
  })
})

// Handle card click - either flip current card or navigate to clicked card
const handleCardClick = (index: number) => {
  if (index === currentIndex.value) {
    // If clicking the current card, toggle its flip state
    cardStates.value[index] = !cardStates.value[index]
  } else {
    // If clicking a different card, navigate to it
    goToCard(index)
  }
}
</script>

<template>
  <div class="relative w-full max-w-4xl flex flex-col justify-center items-center gap-6 h-full mx-auto overflow-visible" :class="class">
    <!-- Cards Container -->
    <div class="relative w-full h-96 flex justify-center items-center">
      <!-- Horizontal Cards -->
      <div v-for="(card, index) in cards" 
           :key="index"
           class="absolute w-80 h-96 transition-all duration-500 ease-in-out -gap-10"
           :style="{
             transform: `translateX(${(index - currentIndex) * 110}%)`,
             zIndex: cards.length - Math.abs(index - currentIndex),
             opacity: index === currentIndex ? 1 : 0.6
           }"
           @click="handleCardClick(index)"
      >
        <FlippingCard 
          :cardData="card"
          :isFlipped="cardStates[index]"
          :canBeFlipped="index === currentIndex"
          class="w-full h-full"
        />
      </div>
    </div>

    <!-- Integrated Navigation Controls -->
    <div class="relative flex flex-col justify-center items-center gap-4">
      <!-- Navigation Container with Arrows and Dots -->
      <div class="flex items-center gap-4 p-4 rounded-2xl neumorphic-concave">
        <!-- Previous Arrow -->
        <button 
          @click="prevCard"
          :disabled="isFirstCard"
          class="w-10 h-10 rounded-full transition-all duration-200 hover:scale-110 disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:scale-100 neumorphic-button"
          :class="isFirstCard 
            ? 'neumorphic-disabled' 
            : 'neumorphic-convex'"
        >
          <span class="text-sm font-bold text-purple-900">←</span>
        </button>

        <!-- Dots -->
        <div class="flex space-x-3">
          <button 
            v-for="(_, index) in cards" 
            :key="index"
            @click="goToCard(index)"
            class="w-4 h-4 rounded-full transition-all duration-200 neumorphic-button"
            :class="currentIndex === index 
              ? 'neumorphic-pressed' 
              : 'neumorphic-convex'"
          />
        </div>

        <!-- Next Arrow -->
        <button 
          @click="nextCard"
          :disabled="isLastCard"
          class="w-10 h-10 rounded-full transition-all duration-200 hover:scale-110 disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:scale-100 neumorphic-button"
          :class="isLastCard 
            ? 'neumorphic-disabled' 
            : 'neumorphic-convex'"
        >
          <span class="text-sm font-bold text-purple-900">→</span>
        </button>
      </div>

      <!-- Card Counter -->
      <div class="px-4 py-2 rounded-xl neumorphic-convex text-purple-900 font-semibold text-sm">
        {{ currentIndex + 1 }} / {{ cards.length }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.neumorphic-button {
  @apply relative;
}

.neumorphic-convex {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.2) 0%, rgba(255, 255, 255, 0.1) 100%);
  box-shadow: 
    8px 8px 16px rgba(0, 0, 0, 0.1),
    -8px -8px 16px rgba(255, 255, 255, 0.1),
    inset -2px -2px 4px rgba(255, 255, 255, 0.05),
    inset 2px 2px 4px rgba(0, 0, 0, 0.05);
}

.neumorphic-concave {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.1) 0%, rgba(255, 255, 255, 0.05) 100%);
  box-shadow: 
    inset 8px 8px 16px rgba(0, 0, 0, 0.1),
    inset -8px -8px 16px rgba(255, 255, 255, 0.1);
}

.neumorphic-pressed {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.05) 0%, rgba(255, 255, 255, 0.1) 100%);
  box-shadow: 
    inset 8px 8px 16px rgba(0, 0, 0, 0.1),
    inset -8px -8px 16px rgba(255, 255, 255, 0.1);
}

.neumorphic-disabled {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.05) 0%, rgba(255, 255, 255, 0.05) 100%);
  box-shadow: 
    inset 2px 2px 4px rgba(0, 0, 0, 0.05),
    inset -2px -2px 4px rgba(255, 255, 255, 0.05);
}
</style> 