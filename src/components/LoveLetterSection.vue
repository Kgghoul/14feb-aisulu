<template>
  <section class="love-letter section" id="letter">
    <div class="letter-wrapper animate-on-scroll" ref="letterRef">
      <div class="letter-paper">
        <div class="letter-ornament letter-ornament-top">❦</div>
        <div class="letter-header">
          <span class="letter-date">14 февраля</span>
        </div>
        <p class="letter-greeting">Дорогая Айсулу,</p>
        <div class="letter-body">
          <p v-for="(char, i) in displayedText" :key="i" v-html="char" class="letter-paragraph"></p>
        </div>
        <div class="letter-signature">
          <p class="letter-closing">твой каримузя 💕</p>
        </div>
        <div class="letter-ornament letter-ornament-bottom">❦</div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const paragraphs = [
  'ⲧы ⳅⲏᥲᥱɯь, ᥴⲕ𐔖᥈ьⲕ𐔖 ρᥲⳅ ⲃ ⲇᥱⲏь я ⲇ𐔤ⲙᥲю 𐔖 ⲧᥱⳝᥱ? ⳝᥱᥴⲕ𐔖ⲏᥱɥⲏ𐔖 ⲙⲏ𐔖𐔏𐔖. ⲕᥲⲿⲇᥲя ⲙыᥴ᥈ь 𐔖 ⲧᥱⳝᥱ ⲏᥲᥒ𐔖᥈ⲏяᥱⲧ ⲙ𐔖ᥱ ᥴᥱρⲇцᥱ ⲧᥱᥒ᥈𐔖ⲙ υ ρᥲⲇ𐔖ᥴⲧью.',
  'ⲧы ⲙ𐔖ύ ᥈𐔤ɥυⲕ ᥴⲃᥱⲧᥲ ⲃ ᥴᥲⲙыᥱ ⲧᥱⲙⲏыᥱ ᥒᥱρυ𐔖ⲇы. ρяⲇ𐔖ⲙ ᥴ ⲧ𐔖ⳝ𐔖ύ я ɥ𐔤ⲃᥴⲧⲃ𐔤ю, ᥴᥱⳝя ᥴᥲⲙыⲙ ᥈юⳝυⲙыⲙ.',
  'я ⳝ᥈ᥲ𐔏𐔖ⲇᥲρᥱⲏ ᥴ𐔤ⲇьⳝᥱ ⳅᥲ ⲕᥲⲿⲇыύ ⲇᥱⲏь, ᥒρ𐔖ⲃᥱⲇᥱⲏⲏыύ ᥴ ⲧ𐔖ⳝ𐔖ύ.',
  '𐔖ⳝᥱպᥲю, ɥⲧ𐔖 ᥴⲕ𐔖ρ𐔖 ⲙы ⳝ𐔤ⲇᥱⲙ ⲃⲙᥱᥴⲧᥱ ᥴ᥈𐔤ɯᥲⲧь ᥴᥱρᥱ𐔏𐔤 ᥒ𐔖 ⲃᥱɥᥱρᥲⲙ υ ᥒυⲧь ⳅᥱⳝρ𐔤 υ᥈υ ⲕ𐔤ɯᥲⲧь ᥒυⲕᥲⳝ𐔤, ⲇᥲ ᥒ𐔖ⲭ ɥⲧ𐔖 ⲕ𐔤ɯᥲⲧь 𐔏᥈ᥲⲃⲏ𐔖ᥱ ɥⲧ𐔖 ⲃⲙᥱᥴⲧᥱ 🌹',
]

const displayedText = ref([])
const letterRef = ref(null)
let observer = null
let isTyping = false

const typeText = async () => {
  if (isTyping) return
  isTyping = true
  
  for (let p = 0; p < paragraphs.length; p++) {
    let currentText = ''
    displayedText.value.push('')
    
    for (let c = 0; c < paragraphs[p].length; c++) {
      currentText += paragraphs[p][c]
      displayedText.value[p] = currentText
      await new Promise(resolve => setTimeout(resolve, 25))
    }
    
    await new Promise(resolve => setTimeout(resolve, 300))
  }
}

onMounted(() => {
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible')
          typeText()
          observer.disconnect()
        }
      })
    },
    { threshold: 0.3 }
  )

  if (letterRef.value) observer.observe(letterRef.value)
})

onUnmounted(() => {
  if (observer) observer.disconnect()
})
</script>

<style scoped>
.love-letter {
  z-index: 2;
  position: relative;
}

.letter-wrapper {
  max-width: 700px;
  width: 100%;
}

.letter-paper {
  background: linear-gradient(145deg, #FFF8F0 0%, #FFEEDD 50%, #FFF5E6 100%);
  border-radius: 8px;
  padding: 50px 45px;
  position: relative;
  box-shadow:
    0 10px 40px rgba(0, 0, 0, 0.3),
    0 0 0 1px rgba(139, 10, 58, 0.1),
    inset 0 0 80px rgba(255, 200, 150, 0.2);
  color: #3D1A1A;
}

.letter-paper::before {
  content: '';
  position: absolute;
  top: 0;
  left: 30px;
  right: 30px;
  bottom: 0;
  background: repeating-linear-gradient(
    transparent,
    transparent 31px,
    rgba(200, 180, 170, 0.2) 31px,
    rgba(200, 180, 170, 0.2) 32px
  );
  pointer-events: none;
}

.letter-ornament {
  text-align: center;
  font-size: 1.8rem;
  color: var(--deep-rose);
  opacity: 0.5;
}

.letter-ornament-top {
  margin-bottom: 20px;
}

.letter-ornament-bottom {
  margin-top: 20px;
  transform: rotate(180deg);
}

.letter-header {
  text-align: right;
  margin-bottom: 25px;
}

.letter-date {
  font-family: var(--font-display);
  font-style: italic;
  font-size: 1.1rem;
  color: #8B6B5A;
}

.letter-greeting {
  font-family: var(--font-display);
  font-size: 1.6rem;
  font-weight: 600;
  color: var(--deep-rose);
  margin-bottom: 25px;
}

.letter-body {
  min-height: 450px;
}

.letter-paragraph {
  font-family: var(--font-display);
  font-size: 1.15rem;
  line-height: 1.9;
  margin-bottom: 18px;
  color: #4A3030;
  position: relative;
}

.letter-paragraph::after {
  content: '|';
  animation: blink 0.8s step-end infinite;
  color: var(--deep-rose);
  font-weight: 100;
}

.letter-paragraph:not(:last-child)::after {
  display: none;
}

@keyframes blink {
  50% { opacity: 0; }
}

.letter-closing {
  font-family: var(--font-display);
  font-size: 1.3rem;
  font-style: italic;
  font-weight: 600;
  color: var(--deep-rose);
  text-align: right;
  margin-top: 30px;
}

@media (max-width: 768px) {
  .letter-paper {
    padding: 30px 25px;
  }

  .letter-greeting {
    font-size: 1.3rem;
  }

  .letter-paragraph {
    font-size: 1rem;
  }
}
</style>
