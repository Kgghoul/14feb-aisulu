<template>
  <div class="app-wrapper">
    <FloatingHearts />
    <HeroSection />
    <PhotoGallery />
    <LoveLetterSection />
    <InteractiveSection />
    <FooterSection />
  </div>
</template>

<script setup>
import FloatingHearts from './components/FloatingHearts.vue'
import HeroSection from './components/HeroSection.vue'
import PhotoGallery from './components/PhotoGallery.vue'
import LoveLetterSection from './components/LoveLetterSection.vue'
import InteractiveSection from './components/InteractiveSection.vue'
import FooterSection from './components/FooterSection.vue'
</script>

<style scoped>
.app-wrapper {
  position: relative;
  background: var(--rose-gradient);
  min-height: 100vh;
}
</style>
