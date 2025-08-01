<script setup lang="ts">
import type { HTMLAttributes } from 'vue'

interface Props {
  class?: HTMLAttributes['class']
  variant?: 'concave' | 'convex' | 'pressed'
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'convex'
})
</script>

<template>
  <div 
    class="relative backdrop-blur-2xl bg-white/10 border border-white/20 rounded-3xl"
    :class="[
      props.class,
      {
        'shadow-concave': variant === 'concave',
        'shadow-convex': variant === 'convex',
        'shadow-pressed': variant === 'pressed'
      }
    ]"
  >
    <slot />
  </div>
</template>

<style scoped>
.shadow-concave {
  box-shadow: 
    inset 8px 8px 16px rgba(255, 255, 255, 0.1),
    inset -8px -8px 16px rgba(0, 0, 0, 0.1),
    8px 8px 16px rgba(0, 0, 0, 0.1),
    -8px -8px 16px rgba(255, 255, 255, 0.1);
}

.shadow-convex {
  box-shadow: 
    8px 8px 16px rgba(0, 0, 0, 0.1),
    -8px -8px 16px rgba(255, 255, 255, 0.1),
    inset -2px -2px 4px rgba(255, 255, 255, 0.05),
    inset 2px 2px 4px rgba(0, 0, 0, 0.05);
}

.shadow-pressed {
  box-shadow: 
    inset 8px 8px 16px rgba(0, 0, 0, 0.1),
    inset -8px -8px 16px rgba(255, 255, 255, 0.1);
}
</style>