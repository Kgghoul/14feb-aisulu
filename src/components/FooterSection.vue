<template>
  <footer class="footer section" id="footer">
    <div class="footer-content">
      <div class="footer-heart" @click="togglePulse">💖</div>
      <h2 class="footer-title">Я люблю тебя, солнышко</h2>
      <div class="footer-stars">
        <span v-for="n in 5" :key="n" class="footer-star" :style="{ animationDelay: n * 0.3 + 's' }">✨</span>
      </div>
      <p class="footer-made">Сделано с ❤️ специально для тебя</p>
    </div>
  </footer>
</template>

<script setup>
import { ref } from 'vue'

const togglePulse = () => {
  // Little easter egg — just a fun click
}
</script>

<style scoped>
.footer {
  z-index: 2;
  position: relative;
  min-height: 60vh;
  background: linear-gradient(180deg, transparent, rgba(74, 0, 32, 0.8));
}

.footer-content {
  text-align: center;
}

.footer-heart {
  font-size: 5rem;
  animation: heartbeat 1.5s ease-in-out infinite;
  cursor: pointer;
  margin-bottom: 30px;
  filter: drop-shadow(0 0 20px rgba(255, 107, 157, 0.5));
  transition: filter 0.3s ease;
}

.footer-heart:hover {
  filter: drop-shadow(0 0 40px rgba(255, 107, 157, 0.8));
}

.footer-title {
  font-family: var(--font-display);
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 700;
  background: linear-gradient(90deg, var(--gold), var(--soft-pink), var(--gold));
  background-size: 200% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 4s linear infinite;
  margin-bottom: 15px;
}

.footer-subtitle {
  font-family: var(--font-display);
  font-size: clamp(1.2rem, 3vw, 1.6rem);
  font-style: italic;
  color: rgba(255, 245, 245, 0.7);
  font-weight: 400;
  margin-bottom: 30px;
}

.footer-stars {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 40px;
}

.footer-star {
  font-size: 1.5rem;
  opacity: 0;
  animation: fadeIn 0.5s ease forwards, float 3s ease-in-out infinite;
}

.footer-made {
  font-size: 0.9rem;
  color: rgba(255, 245, 245, 0.4);
  font-weight: 300;
  letter-spacing: 0.05em;
}

@media (max-width: 768px) {
  .footer-heart {
    font-size: 3.5rem;
  }
}
</style>
