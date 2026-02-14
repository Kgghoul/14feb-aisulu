<template>
  <div class="floating-hearts" aria-hidden="true">
    <div
      v-for="heart in hearts"
      :key="heart.id"
      class="heart"
      :style="heart.style"
    >
      {{ heart.emoji }}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const hearts = ref([])

const emojis = ['💕', '❤️', '💖', '💗', '💓', '🌹', '✨', '💘', '💝']

onMounted(() => {
  const items = []
  for (let i = 0; i < 20; i++) {
    items.push({
      id: i,
      emoji: emojis[Math.floor(Math.random() * emojis.length)],
      style: {
        left: Math.random() * 100 + '%',
        animationDuration: 8 + Math.random() * 12 + 's',
        animationDelay: Math.random() * 10 + 's',
        fontSize: 14 + Math.random() * 20 + 'px',
        opacity: 0.15 + Math.random() * 0.35,
      }
    })
  }
  hearts.value = items
})
</script>

<style scoped>
.floating-hearts {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
  overflow: hidden;
}

.heart {
  position: absolute;
  bottom: -50px;
  animation: riseUp linear infinite;
  will-change: transform;
}

@keyframes riseUp {
  0% {
    transform: translateY(0) rotate(0deg) scale(1);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateY(-110vh) rotate(360deg) scale(0.5);
    opacity: 0;
  }
}
</style>
