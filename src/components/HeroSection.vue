<template>
  <section class="hero section" id="hero">
    <div class="hero-content">
      <div class="hero-sparkle">✨</div>
      <h1 class="hero-title">
        <span class="hero-line hero-line-1">𝓗𝓪𝓹𝓹𝔂 𝓥𝓪𝓵𝓮𝓷𝓽𝓲𝓷𝓮'𝓼</span>
        <span class="hero-line hero-line-2">𝓓𝓪𝔂</span>
      </h1>
      <div class="hero-name">
        <span class="hero-heart">💕</span>
        <span class="hero-aisulu">Бусинка</span>
        <span class="hero-heart">💕</span>
      </div>
      <p class="hero-subtitle">Я тебя оч сильно люблю</p>
      <button class="scroll-btn" @click="scrollDown">
        <span class="scroll-text">листай глупи ↓</span>
      </button>
    </div>

    <!-- Decorative circles -->
    <div class="deco-circle deco-circle-1"></div>
    <div class="deco-circle deco-circle-2"></div>
    <div class="deco-circle deco-circle-3"></div>
  </section>
</template>

<script setup>
const scrollDown = () => {
  const nextSection = document.getElementById('gallery')
  if (nextSection) {
    nextSection.scrollIntoView({ behavior: 'smooth' })
  }
}
</script>

<style scoped>
.hero {
  position: relative;
  z-index: 2;
  overflow: hidden;
  background: transparent;
}

.hero-content {
  text-align: center;
  z-index: 2;
  position: relative;
}

.hero-sparkle {
  font-size: 3rem;
  animation: pulse 2s ease-in-out infinite;
  margin-bottom: 20px;
}

.hero-title {
  font-family: var(--font-display);
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-bottom: 30px;
}

.hero-line {
  display: block;
  opacity: 0;
  animation: fadeInUp 1s ease forwards;
}

.hero-line-1 {
  font-size: clamp(2.2rem, 6vw, 4.5rem);
  font-weight: 600;
  color: var(--warm-white);
  animation-delay: 0.3s;
  letter-spacing: 0.05em;
}

.hero-line-2 {
  font-size: clamp(3rem, 8vw, 6rem);
  font-weight: 700;
  background: linear-gradient(90deg, var(--gold), var(--gold-light), var(--gold));
  background-size: 200% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: fadeInUp 1s ease 0.6s forwards, shimmer 3s linear 1.6s infinite;
  filter: drop-shadow(0 0 30px rgba(255, 215, 0, 0.3));
}

.hero-name {
  opacity: 0;
  animation: fadeInUp 1s ease 1s forwards;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
}

.hero-aisulu {
  font-family: var(--font-display);
  font-size: clamp(2.5rem, 7vw, 5rem);
  font-weight: 700;
  font-style: italic;
  color: var(--soft-pink);
  text-shadow: 0 0 40px rgba(255, 107, 157, 0.5);
}

.hero-heart {
  font-size: 2rem;
  animation: heartbeat 1.5s ease-in-out infinite;
}

.hero-subtitle {
  font-family: var(--font-body);
  font-size: clamp(1rem, 2.5vw, 1.4rem);
  font-weight: 300;
  color: rgba(255, 245, 245, 0.8);
  opacity: 0;
  animation: fadeInUp 1s ease 1.4s forwards;
  margin-bottom: 50px;
  letter-spacing: 0.03em;
}

/* Scroll Button */
.scroll-btn {
  background: none;
  border: 2px solid var(--gold);
  color: var(--gold);
  padding: 12px 30px;
  border-radius: 50px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  opacity: 0;
  animation: fadeIn 1s ease 2s forwards, float 2s ease-in-out 3s infinite;
  transition: background 0.3s ease, transform 0.3s ease;
}

.scroll-btn:hover {
  background: var(--gold);
  color: var(--dark-wine);
  transform: translateY(-3px);
}

.scroll-text {
  font-family: var(--font-body);
  font-size: 1rem;
  font-weight: 500;
  letter-spacing: 0.05em;
}

/* Decorative Circles */
.deco-circle {
  position: absolute;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(255, 107, 157, 0.15), transparent 70%);
  pointer-events: none;
}

.deco-circle-1 {
  width: 400px;
  height: 400px;
  top: -100px;
  right: -100px;
  animation: float 6s ease-in-out infinite;
}

.deco-circle-2 {
  width: 300px;
  height: 300px;
  bottom: -50px;
  left: -80px;
  animation: float 8s ease-in-out 2s infinite;
}

.deco-circle-3 {
  width: 200px;
  height: 200px;
  top: 40%;
  right: 10%;
  background: radial-gradient(circle, rgba(255, 215, 0, 0.1), transparent 70%);
  animation: float 7s ease-in-out 1s infinite;
}

@media (max-width: 768px) {
  .hero-name {
    gap: 10px;
  }
  .hero-heart {
    font-size: 1.5rem;
  }
}
</style>
