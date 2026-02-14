<template>
  <section class="interactive section" id="interactive">
    <div class="interactive-content">
      <h2 class="section-title animate-on-scroll" ref="titleRef">
        сколько мы уже встр?
      </h2>

      <!-- Counter -->
      <div class="counter-block animate-on-scroll" ref="counterRef">
        <p class="counter-label">Мы вместе уже</p>
        <div class="counter-number" :key="formatIndex">
          <template v-if="currentDisplay.parts">
            <span v-for="(part, i) in currentDisplay.parts" :key="i" class="counter-part">
              <span class="counter-value">{{ part.value }}</span>
              <span class="counter-unit">{{ part.unit }}</span>
            </span>
          </template>
        </div>
      </div>

      <!-- Format Switch Button -->
      <div class="explosion-block animate-on-scroll" ref="btnRef">
        <p class="explosion-label">Нажми, чтобы сменить формат:</p>
        <button class="love-btn" @click="switchFormat">
          <span class="love-btn-emoji">💖</span>
          <span class="love-btn-text">{{ formatLabels[nextFormatIndex] }}</span>
        </button>
      </div>

      <!-- Explosion Hearts -->
      <div class="explosion-container" ref="explosionRef"></div>
      <button class="next-btn" @click="scrollNext">
        дальше
      </button>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Set your relationship start date here
const startDate = new Date('2022-02-21')
const now = new Date()
const diffTime = Math.abs(now - startDate)
const totalDays = Math.floor(diffTime / (1000 * 60 * 60 * 24))

// Russian declension helper
const declension = (n, one, few, many) => {
  const lastTwo = n % 100
  const lastOne = n % 10
  if (lastTwo >= 11 && lastTwo <= 19) return many
  if (lastOne === 1) return one
  if (lastOne >= 2 && lastOne <= 4) return few
  return many
}

// Calculate full breakdown
let calcMonths = (now.getFullYear() - startDate.getFullYear()) * 12 + (now.getMonth() - startDate.getMonth())
if (now.getDate() < startDate.getDate()) {
  calcMonths--
}
const totalMonths = calcMonths
const totalYears = Math.floor(totalMonths / 12)
const remainderMonths = totalMonths % 12
const totalWeeks = Math.floor(totalDays / 7)
const totalHours = totalDays * 24

// Remainder days calc
const tempDate = new Date(startDate)
tempDate.setFullYear(tempDate.getFullYear() + totalYears)
tempDate.setMonth(tempDate.getMonth() + remainderMonths)
const remainderDays = Math.floor((now - tempDate) / (1000 * 60 * 60 * 24))

const formatIndex = ref(0)

const formats = computed(() => [
  // 0: Дни
  {
    parts: [{ value: totalDays, unit: declension(totalDays, 'день', 'дня', 'дней') }]
  },
  // 1: Недели
  {
    parts: [{ value: totalWeeks, unit: declension(totalWeeks, 'неделю', 'недели', 'недель') }]
  },
  // 2: Месяцы
  {
    parts: [{ value: totalMonths, unit: declension(totalMonths, 'месяц', 'месяца', 'месяцев') }]
  },
  // 3: Годы
  {
    parts: [{ value: totalYears, unit: declension(totalYears, 'год', 'года', 'лет') }]
  },
  // 4: Годы + месяцы + дни
  {
    parts: [
      { value: totalYears, unit: declension(totalYears, 'год', 'года', 'лет') },
      { value: remainderMonths, unit: declension(remainderMonths, 'месяц', 'месяца', 'месяцев') },
      { value: remainderDays, unit: declension(remainderDays, 'день', 'дня', 'дней') },
    ]
  },
  // 5: Часы
  {
    parts: [{ value: totalHours.toLocaleString(), unit: declension(totalHours, 'час', 'часа', 'часов') }]
  },
])

const formatLabels = ['В днях', 'В неделях', 'В месяцах', 'В годах', 'Полный формат', 'В часах']

const currentDisplay = computed(() => formats.value[formatIndex.value])
const nextFormatIndex = computed(() => (formatIndex.value + 1) % formats.value.length)

const explosionRef = ref(null)
const titleRef = ref(null)
const counterRef = ref(null)
const btnRef = ref(null)
let observer = null

const switchFormat = () => {
  formatIndex.value = (formatIndex.value + 1) % formats.value.length

  // Also trigger heart explosion
  const container = explosionRef.value
  if (!container) return

  // Create 40 hearts
  for (let i = 0; i < 40; i++) {
    const heart = document.createElement('div')
    heart.classList.add('explosion-heart')
    heart.innerHTML = '❤️'
    heart.style.left = '50%'
    heart.style.top = '50%'
    const angle = Math.random() * Math.PI * 2
    const velocity = 100 + Math.random() * 200
    const tx = Math.cos(angle) * velocity
    const ty = Math.sin(angle) * velocity
    heart.style.setProperty('--tx', `${tx}px`)
    heart.style.setProperty('--ty', `${ty}px`)
    heart.style.animation = `explode 1s ease-out forwards`
    
    container.appendChild(heart)
    setTimeout(() => heart.remove(), 1000)
  }
}

const scrollNext = () => {
  const section = document.getElementById('footer')
  if (section) section.scrollIntoView({ behavior: 'smooth' })
}

onMounted(() => {
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible')
        }
      })
    },
    { threshold: 0.1 }
  )

  ;[titleRef, counterRef, btnRef].forEach(r => {
    if (r.value) observer.observe(r.value)
  })
})

onUnmounted(() => {
  if (observer) observer.disconnect()
})
</script>

<style scoped>
.interactive {
  z-index: 2;
  position: relative;
  background: linear-gradient(180deg, transparent, rgba(74, 0, 32, 0.3), transparent);
}

.interactive-content {
  text-align: center;
  max-width: 700px;
  width: 100%;
  position: relative;
}

/* Counter */
.counter-block {
  margin-bottom: 60px;
}

.counter-label {
  font-family: var(--font-display);
  font-size: 1.4rem;
  color: rgba(255, 245, 245, 0.8);
  margin-bottom: 15px;
}

.counter-number {
  display: flex;
  align-items: baseline;
  justify-content: center;
  gap: 15px;
  flex-wrap: wrap;
}

.counter-part {
  display: inline-flex;
  align-items: baseline;
  gap: 8px;
}

.counter-value {
  font-family: var(--font-display);
  font-size: clamp(4rem, 10vw, 7rem);
  font-weight: 700;
  background: linear-gradient(135deg, var(--gold), var(--soft-pink), var(--gold));
  background-size: 200% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 3s linear infinite;
  line-height: 1;
}

.counter-unit {
  font-family: var(--font-display);
  font-size: clamp(1.5rem, 4vw, 2.5rem);
  color: var(--gold);
  font-weight: 600;
}

.counter-sub {
  font-size: 1.1rem;
  color: rgba(255, 245, 245, 0.7);
  margin-top: 15px;
  font-weight: 300;
}

/* Explosion Button */
.explosion-block {
  position: relative;
}

.explosion-label {
  font-size: 1.1rem;
  color: rgba(255, 245, 245, 0.8);
  margin-bottom: 25px;
  font-weight: 300;
}

.love-btn {
  background: linear-gradient(135deg, var(--soft-pink), var(--deep-rose));
  border: 2px solid rgba(255, 215, 0, 0.3);
  color: var(--warm-white);
  padding: 18px 45px;
  border-radius: 50px;
  font-family: var(--font-body);
  font-size: 1.15rem;
  font-weight: 500;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 12px;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.love-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.15), transparent);
  transition: left 0.5s ease;
}

.love-btn:hover::before {
  left: 100%;
}

.love-btn:hover {
  transform: scale(1.05);
  border-color: var(--gold);
  box-shadow: 0 10px 30px rgba(255, 107, 157, 0.4);
}

.love-btn:active {
  transform: scale(0.95);
}

.love-btn-emoji {
  font-size: 1.5rem;
  animation: heartbeat 1.5s ease-in-out infinite;
}

/* Explosion Container */
.explosion-container {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  pointer-events: none;
  z-index: 100;
}

:deep(.explosion-heart) {
  position: absolute;
  animation: explode 1.5s ease-out forwards;
  pointer-events: none;
}

@keyframes explode {
  0% {
    transform: translate(0, 0) rotate(0deg) scale(0);
    opacity: 1;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform:
      translate(var(--x), var(--y))
      rotate(var(--r))
      scale(var(--s));
    opacity: 0;
  }
}

@media (max-width: 768px) {
  .love-btn {
    padding: 15px 35px;
    font-size: 1rem;
    white-space: nowrap;
  }
}

.next-btn {
  margin-top: 40px;
  background: none;
  border: 1px solid var(--gold);
  color: var(--gold);
  padding: 10px 25px;
  border-radius: 50px;
  cursor: pointer;
  font-family: var(--font-body);
  font-size: 0.9rem;
  letter-spacing: 0.05em;
  transition: all 0.3s ease;
}

.next-btn:hover {
  background: var(--gold);
  color: var(--dark-wine);
  transform: translateY(-2px);
}
</style>
