<template>
  <div class="min-h-screen bg-gradient-to-br from-red-100 via-green-100 to-red-100">
    <!-- Yılbaşı Elementleri Arka Planı -->
    <div class="fixed inset-0 pointer-events-none overflow-hidden">
      <div v-for="item in festiveItems" :key="item.id" class="absolute animate-float opacity-30" :style="{
        left: `${item.x}%`,
        top: `${item.y}%`,
        animationDelay: `${item.delay}s`,
        fontSize: `${item.size}px`
      }">
        {{ item.emoji }}
      </div>
    </div>

    <!-- Ana İçerik -->
    <div class="container mx-auto px-4 py-12">
      <!-- Header -->
      <header class="text-center mb-16">
        <h1
          class="text-6xl font-bold bg-gradient-to-r from-red-600 to-green-600 text-transparent bg-clip-text animate-pulse-slow">
          Benim Tatlı Sinirli Meleğim
        </h1>
        <p class="mt-4 text-xl text-gray-600">Kaşları çatık ama kalbi altın 💝</p>
      </header>

      <!-- Sinirlilik Ölçer -->
      <div class="card text-center mb-16 max-w-md mx-auto">
        <h2 class="text-2xl font-bold text-red-600 mb-4">Sinir Ölçer 😤</h2>
        <p class="mb-4">Sinir seviyesi şu an:</p>
        <div class="text-8xl cursor-pointer mb-4" @click="handleAngerLevel">
          <div :class="{ 'animate-bounce': isAngryAnimating }"
            class="transform hover:scale-110 transition-transform inline-block">
            {{ currentMood }}
          </div>
        </div>
        <p class="text-gray-600">{{ getAngerMessage() }}</p>
      </div>

      <!-- Nida Mood Metre -->
      <div class="card mb-16">
        <h2 class="text-2xl font-bold text-green-600 mb-4">Nida'nın Yılbaşı Mood'u</h2>
        <div class="space-y-4">
          <div v-for="(mood, index) in moodStats" :key="index" class="flex items-center justify-between">
            <span class="text-gray-700">{{ mood.label }}</span>
            <div class="w-48 bg-gray-200 rounded-full h-4">
              <div class="h-full rounded-full bg-gradient-to-r from-red-500 to-green-500"
                :style="{ width: `${mood.value}%` }" :class="{ 'animate-pulse': mood.value > 80 }">
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Mesajlar ve Senaryolar -->
      <div class="grid md:grid-cols-2 gap-8 mb-16">
        <div class="card">
          <h2 class="text-2xl font-bold text-red-600 mb-4">Günün Nida Gerçeği</h2>
          <div class="h-32 flex items-center justify-center text-xl px-4">
            {{ currentMessage }}
          </div>
          <button @click="changeMessage" class="love-button mt-4 w-full">
            Yeni Mesaj 🎄
          </button>
        </div>

        <!-- Hayali Senaryolar -->
        <div class="card">
          <h2 class="text-2xl font-bold text-green-600 mb-4">Bugünkü Senaryo</h2>
          <div class="h-32 flex items-center justify-center text-xl px-4">
            {{ currentScenario }}
          </div>
          <button @click="changeScenario" class="love-button mt-4 w-full">
            Yeni Senaryo 🎭
          </button>
        </div>
      </div>

      <!-- Nida Sözlüğü -->
      <div class="grid md:grid-cols-3 gap-6">
        <div v-for="(entry, index) in nidaDictionary" :key="index" class="card text-center cursor-pointer"
          @click="revealDefinition(index)">
          <div class="h-40 flex items-center justify-center px-4">
            <div>
              <h3 class="font-bold text-red-600 mb-2">{{ entry.word }}</h3>
              <p class="text-lg" :class="{ 'text-gray-700': entry.isRevealed, 'text-red-500': !entry.isRevealed }">
                {{ entry.isRevealed ? entry.definition : 'Tanımı görmek için tıkla' }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

// Yılbaşı elementleri
const festiveItems = ref([
  { id: 1, emoji: '🎄', x: 10, y: 20, delay: 0, size: 24 },
  { id: 2, emoji: '🎅', x: 80, y: 40, delay: 1, size: 28 },
  { id: 3, emoji: '🎁', x: 30, y: 70, delay: 2, size: 32 },
  { id: 4, emoji: '⭐', x: 70, y: 30, delay: 1.5, size: 20 },
  { id: 5, emoji: '🦌', x: 20, y: 85, delay: 2.5, size: 26 },
  { id: 6, emoji: '❄️', x: 60, y: 15, delay: 3, size: 22 },
  { id: 7, emoji: '🔔', x: 40, y: 60, delay: 2.8, size: 24 },
])

// Sinir ölçer için değişkenler
const angerLevel = ref(0)
const isAngryAnimating = ref(false)
const currentMood = ref('😊')
let angerTimer = null

const moodEmojis = ['😊', '😐', '😤', '😠', '🤬']

const handleAngerLevel = () => {
  angerLevel.value = (angerLevel.value + 1) % moodEmojis.length
  currentMood.value = moodEmojis[angerLevel.value]
  isAngryAnimating.value = true
  setTimeout(() => {
    isAngryAnimating.value = false
  }, 1000)
}

const getAngerMessage = () => {
  const messages = [
    "Sakin bir melek 👼",
    "Hafif gergin 😅",
    "Kaşlar çatılmaya başladı 🤨",
    "Tam Nida modunda 😠",
    "DİKKAT: Maksimum sinir seviyesi 🚨"
  ]
  return messages[angerLevel.value]
}

// Mood istatistikleri
const moodStats = ref([
  { label: 'Sinir Seviyesi', value: 85 },
  { label: 'Kafada Kurma Seviyesi', value: 95 },
  { label: 'Senaryo Yazarlığı', value: 98 },
  { label: 'Kaş Çatıklık Oranı', value: 90 },
  { label: 'Tatlılık Seviyesi', value: 100 },
])

// Esprili mesajlar
const loveMessages = [
  "Kaşların çatık ama kalbim sana açık! 💝",
  "Senin kafanda kurulan senaryolar Hollywood'u geçti valla! 🎬",
  "Sinirlenince kaşların milimetrik açıyla yükseliyor, matematik dehası! 📐",
  "Sen kaş çatınca melekler bile 'Aman dikkat!' diyor 😇",
  "Yılbaşı ağacından daha parlak gözlerin var, sinirlenince bile! ✨",
  "Kaş çatma şampiyonluğunu kimseye kaptırmıyorsun! 🏆",
  "Senin bir 'off ya!' deyişin = 1000 şiir 📝"
]

// Senaryo örnekleri
const scenarios = [
  "Bugünkü senaryo: 'Kesin beni düşünmüyordur!' (24/7 aklımdasın 💭)",
  "Kafadaki film: 'Ya aslında sürpriz hazırlıyorsa?' (Yoo, sadece uyuyorum 😴)",
  "Günün senaryosu: 'Kesin başka planları var!' (Evet, seninle film izlemek 🎬)",
  "Bugünün kurgusu: 'Off kesin unuttu!' (Asla unutmam, sadece takvime bakmam gerek 📅)"
]

// Nida Sözlüğü
const nidaDictionary = ref([
  {
    word: "Nida'ca Sinirlenme",
    definition: "Dünyalar tatlısı bir kızın kaşlarını çatıp sevimli görünme sanatı 😤",
    isRevealed: false
  },
  {
    word: "Senaryo Kurma",
    definition: "Hollywood'a taş çıkartan hayal gücü ile olayları analiz etme yeteneği 🎭",
    isRevealed: false
  },
  {
    word: "Off Ya!",
    definition: "Nida dilinde: Dünyanın en sevimli kızgınlık ifadesi 💝",
    isRevealed: false
  },
])

const currentMessage = ref('')
const currentScenario = ref('')

const changeMessage = () => {
  let newIndex
  do {
    newIndex = Math.floor(Math.random() * loveMessages.length)
  } while (loveMessages[newIndex] === currentMessage.value)
  currentMessage.value = loveMessages[newIndex]
}

const changeScenario = () => {
  let newIndex
  do {
    newIndex = Math.floor(Math.random() * scenarios.length)
  } while (scenarios[newIndex] === currentScenario.value)
  currentScenario.value = scenarios[newIndex]
}

const revealDefinition = (index) => {
  nidaDictionary.value[index].isRevealed = true
}

onMounted(() => {
  changeMessage()
  changeScenario()
})

onBeforeUnmount(() => {
  if (angerTimer) {
    clearTimeout(angerTimer)
  }
})
</script>

<style>
.love-button {
  @apply px-6 py-3 bg-gradient-to-r from-red-500 to-green-500 text-white rounded-full font-semibold transform transition-all duration-300 hover:scale-105 hover:shadow-lg active:scale-95 focus:outline-none;
}

.card {
  @apply bg-white/80 backdrop-blur-sm rounded-xl shadow-xl p-6 transition-all duration-300 hover:shadow-2xl hover:scale-[1.02];
}

@keyframes float {

  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-20px);
  }
}

.animate-float {
  animation: float 3s ease-in-out infinite;
}
</style>