<template>
  <section class="gallery section" id="gallery">
    <h2 class="section-title animate-on-scroll" ref="titleRef">💕 мы?</h2>

    <div class="gallery-stage">
      <!-- Main heart photo -->
      <div class="heart-container">
        <div class="heart-shape">
          <transition name="photo-fade" mode="out-in">
            <img
              :src="photos[currentIndex]"
              :key="currentIndex"
              class="heart-photo"
              alt="Наше фото"
            />
          </transition>
        </div>
        <!-- Glow behind heart -->
        <div class="heart-glow"></div>
      </div>

      <!-- Dots navigation -->
      <div class="gallery-dots">
        <button
          v-for="(photo, i) in photos"
          :key="i"
          class="gallery-dot"
          :class="{ active: i === currentIndex }"
          @click="goTo(i)"
        ></button>
      </div>

      <!-- Small floating mini hearts with other photos -->
      <div class="mini-hearts">
        <div
          v-for="(mh, i) in miniHearts"
          :key="i"
          class="mini-heart"
          :style="mh.style"
        >
          <img :src="mh.photo" alt="" class="mini-heart-img" />
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const photos = [
  '/photos/photo_5226610818961575977_y.jpg',
  '/photos/photo_5226610818961576138_y.jpg',
  '/photos/photo_5226610818961576152_y.jpg',
  '/photos/photo_5226610818961576157_y.jpg',
  '/photos/photo_5226610818961576158_y.jpg',
  '/photos/photo_5226610818961576159_x.jpg',
  '/photos/photo_5226610818961576172_y.jpg',
]

const currentIndex = ref(0)
let interval = null
const titleRef = ref(null)
let observer = null

const goTo = (i) => {
  currentIndex.value = i
  resetInterval()
}

const nextPhoto = () => {
  currentIndex.value = (currentIndex.value + 1) % photos.length
}

const resetInterval = () => {
  if (interval) clearInterval(interval)
  interval = setInterval(nextPhoto, 3500)
}

// Mini floating hearts with photos
const miniHearts = computed(() => {
  const positions = [
    { top: '5%', left: '8%', size: 60, delay: 0 },
    { top: '15%', right: '10%', size: 50, delay: 1.5 },
    { bottom: '20%', left: '5%', size: 45, delay: 3 },
    { bottom: '10%', right: '8%', size: 55, delay: 0.8 },
    { top: '45%', left: '2%', size: 40, delay: 2.2 },
  ]
  return positions.map((pos, i) => ({
    photo: photos[(currentIndex.value + i + 1) % photos.length],
    style: {
      top: pos.top,
      left: pos.left,
      right: pos.right,
      bottom: pos.bottom,
      width: pos.size + 'px',
      height: pos.size + 'px',
      animationDelay: pos.delay + 's',
    }
  }))
})

onMounted(() => {
  resetInterval()

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) entry.target.classList.add('visible')
      })
    },
    { threshold: 0.1 }
  )
  if (titleRef.value) observer.observe(titleRef.value)
})

onUnmounted(() => {
  if (interval) clearInterval(interval)
  if (observer) observer.disconnect()
})
</script>

<style scoped>
.gallery {
  z-index: 2;
  position: relative;
  background: linear-gradient(180deg, transparent, rgba(74, 0, 32, 0.5), transparent);
  overflow: hidden;
}

.gallery-stage {
  position: relative;
  width: 100%;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Heart shape via clip-path */
.heart-container {
  position: relative;
  width: 320px;
  height: 300px;
  margin-bottom: 30px;
}

.heart-shape {
  width: 100%;
  height: 100%;
  clip-path: path('M160 280 C160 280 10 200 10 120 C10 60 60 20 110 20 C135 20 155 35 160 55 C165 35 185 20 210 20 C260 20 310 60 310 120 C310 200 160 280 160 280Z');
  overflow: hidden;
  position: relative;
  z-index: 2;
  filter: drop-shadow(0 10px 30px rgba(139, 10, 58, 0.5));
}

.heart-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.heart-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 350px;
  height: 350px;
  background: radial-gradient(circle, rgba(255, 107, 157, 0.3) 0%, transparent 70%);
  z-index: 1;
  animation: pulse 3s ease-in-out infinite;
}

/* Photo transition */
.photo-fade-enter-active {
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.photo-fade-leave-active {
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.photo-fade-enter-from {
  opacity: 0;
  transform: scale(1.08);
}

.photo-fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

/* Navigation dots */
.gallery-dots {
  display: flex;
  gap: 10px;
  z-index: 3;
}

.gallery-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 2px solid var(--gold);
  background: transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 0;
}

.gallery-dot.active {
  background: var(--gold);
  transform: scale(1.3);
  box-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
}

.gallery-dot:hover {
  background: rgba(255, 215, 0, 0.5);
}

/* Mini floating hearts */
.mini-hearts {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 1;
}

.mini-heart {
  position: absolute;
  clip-path: path('M25 45 C25 45 2 32 2 19 C2 9 9 3 17 3 C21 3 24 5 25 9 C26 5 29 3 33 3 C41 3 48 9 48 19 C48 32 25 45 25 45Z');
  opacity: 0.6;
  animation: float 4s ease-in-out infinite;
  overflow: hidden;
  filter: drop-shadow(0 4px 10px rgba(139, 10, 58, 0.4));
  transition: opacity 0.5s ease;
}

.mini-heart-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

@media (max-width: 768px) {
  .heart-container {
    width: 250px;
    height: 235px;
  }

  .heart-shape {
    clip-path: path('M125 220 C125 220 8 156 8 94 C8 47 47 16 86 16 C106 16 121 27 125 43 C129 27 144 16 164 16 C203 16 242 47 242 94 C242 156 125 220 125 220Z');
  }

  .mini-heart {
    display: none;
  }
}
</style>
