<script setup lang="ts">
import type { HTMLAttributes } from 'vue'

interface Props {
  customClass?: HTMLAttributes['class']
}

defineProps<Props>()
</script>

<template>
  <div class="min-h-screen w-full relative overflow-hidden grainy-bg" :class="customClass">
    <slot />
  </div>
</template>

<style scoped>
.grainy-bg {
  background: hsla(238, 100%, 71%, 1);
  background: radial-gradient(circle, hsla(238, 100%, 71%, 1) 0%, hsla(295, 100%, 84%, 1) 100%);
  background: -moz-radial-gradient(circle, hsla(238, 100%, 71%, 1) 0%, hsla(295, 100%, 84%, 1) 100%);
  background: -webkit-radial-gradient(circle, hsla(238, 100%, 71%, 1) 0%, hsla(295, 100%, 84%, 1) 100%);
  filter: progid: DXImageTransform.Microsoft.gradient( startColorstr="#696EFF", endColorstr="#F8ACFF", GradientType=0 );
}

.grainy-bg:before {
  content: "";
  background-color: transparent;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 600 600'%3E%3Cfilter id='a'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23a)'/%3E%3C/svg%3E");
  background-repeat: repeat;
  background-size: 182px;
  opacity: 0.12;
  top: 0;
  left: 0;
  position: absolute;
  width: 100%;
  height: 100%;
}
</style> 