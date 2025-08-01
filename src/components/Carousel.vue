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
  <div class="relative w-full max-w-4xl mx-auto overflow-visible" :class="class">
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

    <!-- Navigation Arrows -->
    <button 
      @click="prevCard"
      :disabled="isFirstCard"
      class="absolute -left-10 top-1/2 -translate-y-1/2 p-3 rounded-full transition-all duration-200 hover:scale-110 disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:scale-100 z-10"
      :class="isFirstCard 
        ? 'bg-white/10 text-white/50' 
        : 'bg-white/20 backdrop-blur-sm hover:bg-white/30 text-white'"
    >
      ←
    </button>
    <button 
      @click="nextCard"
      :disabled="isLastCard"
      class="absolute -right-10 top-1/2 -translate-y-1/2 p-3 rounded-full transition-all duration-200 hover:scale-110 disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:scale-100 z-10"
      :class="isLastCard 
        ? 'bg-white/10 text-white/50' 
        : 'bg-white/20 backdrop-blur-sm hover:bg-white/30 text-white'"
    >
      →
    </button>

    <!-- Dots Indicator -->
    <div class="absolute -bottom-24 left-1/2 -translate-x-1/2 flex flex-col justify-center items-center gap-4">
      <div class="flex space-x-2">
        <button 
          v-for="(_, index) in cards" 
          :key="index"
          @click="goToCard(index)"
          class="w-3 h-3 rounded-full transition-all duration-200"
          :class="currentIndex === index 
            ? 'bg-white scale-125' 
            : 'bg-white/40 hover:bg-white/60'"
        />
      </div>
      <!-- Card Counter -->
      <div class=" text-white/80 text-sm bg-black/20 backdrop-blur-sm px-3 py-1 rounded-full w-fit">
        {{ currentIndex + 1 }} / {{ cards.length }}
      </div>
    </div>
  </div>
</template> 