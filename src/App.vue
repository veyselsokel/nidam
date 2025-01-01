<template>
  <div class="min-h-screen bg-gradient-to-br from-red-200 via-green-200 to-red-200">
    <!-- Yılbaşı Elementleri Arka Planı -->
    <div class="fixed inset-0 pointer-events-none overflow-hidden">
      <div v-for="item in festiveItems" :key="item.id" class="absolute animate-float opacity-40" :style="{
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
        <h1 class="text-6xl font-bold text-red-700 animate-pulse-slow drop-shadow-lg">
          Benim Tatlı Sinirli Meleğim
        </h1>
        <p class="mt-4 text-2xl text-gray-800 font-semibold">Kaşları çatık ama kalbi altın 💝</p>
      </header>

      <!-- Yeni Yıl Şiiri -->
      <div class="card bg-white/90 mb-16 max-w-2xl mx-auto text-center">
        <h2 class="text-3xl font-bold text-red-700 mb-6">Yeni Yılda Sen</h2>
        <div class="prose prose-lg mx-auto text-gray-800 italic">
          <p>
            Her yeni yılda açan çiçek gibi,<br />
            Gözlerindeki ışıltı büyür gecede.<br />
            Kızgın görünen o tatlı hallerin,<br />
            En değerli hazinem bu senede.
          </p>
          <p class="mt-4">
            Kaş çatışın, gülüşün, her halin,<br />
            Renk katar dünyama her an yeniden.<br />
            Sen varsan, her mevsim bahar,<br />
            Her gün yılbaşı seninle benim için.
          </p>
        </div>
      </div>

      <!-- Sinirlilik Ölçer -->
      <div class="card bg-white/90 text-center mb-16 max-w-md mx-auto">
        <h2 class="text-2xl font-bold text-red-700 mb-4">Yeni Yıl Sinir Ölçer 🎄</h2>
        <p class="mb-4 text-gray-800">Yeni yıl sinir seviyesi:</p>
        <div class="text-8xl cursor-pointer mb-4" @click="handleAngerLevel">
          <div :class="{ 'animate-bounce': isAngryAnimating }"
            class="transform hover:scale-110 transition-transform inline-block">
            {{ currentMood }}
          </div>
        </div>
        <p class="text-gray-800 font-medium">{{ getAngerMessage() }}</p>
      </div>

      <!-- Nida Mood Metre -->
      <div class="card bg-white/90 mb-16">
        <h2 class="text-2xl font-bold text-green-700 mb-4">Nida'nın Yılbaşı Mood'u</h2>
        <div class="space-y-4">
          <div v-for="(mood, index) in moodStats" :key="index" class="flex items-center justify-between">
            <span class="text-gray-800 font-medium">{{ mood.label }}</span>
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
        <div class="card bg-white/90">
          <h2 class="text-2xl font-bold text-red-700 mb-4">Falan Filan</h2>
          <div class="h-32 flex items-center justify-center text-xl px-4 text-gray-800">
            {{ currentMessage }}
          </div>
          <button @click="changeMessage" class="love-button mt-4 w-full">
            Yeni Mesaj 🎄
          </button>
        </div>

        <div class="card bg-white/90">
          <h2 class="text-2xl font-bold text-green-700 mb-4">Edebiyat Parçalamaca</h2>
          <div class="h-32 flex items-center justify-center text-xl px-4 text-gray-800">
            {{ currentScenario }}
          </div>
          <button @click="changeScenario" class="love-button mt-4 w-full">
            Yeni Senaryo 🎭
          </button>
        </div>
      </div>

      <!-- Nida Sözlüğü -->
      <div class="grid md:grid-cols-3 gap-6">
        <div v-for="(entry, index) in nidaDictionary" :key="index"
          class="card bg-white/90 text-center cursor-pointer hover:bg-white/95" @click="revealDefinition(index)">
          <div class="h-40 flex items-center justify-center px-4">
            <div>
              <h3 class="font-bold text-red-700 mb-2">{{ entry.word }}</h3>
              <p class="text-lg" :class="{ 'text-gray-800': entry.isRevealed, 'text-red-600': !entry.isRevealed }">
                {{ entry.isRevealed ? entry.definition : 'Tanımı görmek için tıkla' }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

// Yılbaşı elementleri - daha fazla eklendi
const festiveItems = ref([
  { id: 1, emoji: '🎄', x: 10, y: 20, delay: 0, size: 24 },
  { id: 2, emoji: '🎅', x: 80, y: 40, delay: 1, size: 28 },
  { id: 3, emoji: '🎁', x: 30, y: 70, delay: 2, size: 32 },
  { id: 4, emoji: '⭐', x: 70, y: 30, delay: 1.5, size: 20 },
  { id: 5, emoji: '🦌', x: 20, y: 85, delay: 2.5, size: 26 },
  { id: 6, emoji: '❄️', x: 60, y: 15, delay: 3, size: 22 },
  { id: 7, emoji: '🔔', x: 40, y: 60, delay: 2.8, size: 24 },
  { id: 8, emoji: '💝', x: 85, y: 75, delay: 3.2, size: 28 },
  { id: 9, emoji: '✨', x: 15, y: 45, delay: 2.3, size: 20 },
  { id: 10, emoji: '🕊️', x: 75, y: 25, delay: 1.8, size: 26 }
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
    "Yeni yılın en tatlı meleği 👼",
    "Minik bir yılbaşı gerginliği 😅",
    "Geliyo 🤨",
    "Nida modunda (kaşının çizgisini yerim) 😠",
    "Veysel bişeyi unuttu 🚨"
  ]
  return messages[angerLevel.value]
}

// Mood istatistikleri - yılbaşı temalı güncellendi
const moodStats = ref([
  { label: 'Kafada Kurma', value: 95 },
  { label: 'Senaryo Yazma', value: 98 },
  { label: 'Kaş Çatma', value: 85 },
  { label: 'Tatlılık Seviyesi', value: 100 },
  { label: 'Can Çıkıklığı', value: 110 },
  { label: 'Libido ;)', value: 198 },
])

// Esprili mesajlar - yılbaşı temalı güncellendi
const loveMessages = [
  "Bu yılbaşı hediyem sensin, başka hediye istemem! 🎁",
  "Senayo yazmaya devam 🎬",
  "Kafada kurmaya devam 🎬",
  "Seninle her gün yılbaşı 🎄",
  "Götünü yerim! 🍑",
  "Ay'dan daha parlak gözlerin var! ✨",
  "Hiç ter kokmuyorsun 🤔",
  "Kaşının çizgisini yerim! 🏆",
]

// Senaryo örnekleri - yılbaşı temalı güncellendi
const scenarios = [
  "Sevgi insanlığın görünmeyen yüzüdür.",
  "Aşk, sevdiğinin mutluluğunu kendi mutluluğuna tercih etmektir.",
  "Ben senin; sevgilin, eşin, baban, ağabeyin, arkadaşınım. Biri bitse biri kalır. Seni hiç bırakmayacağım.",
  "Sesinden öpüyorum, yüreğime serçeler bırakan sesinden…",
  "Ne kadar seviyorsun dersen; 'Nar' kadar derim.Dışımdan bir ben görünürüm, içimden binlerce sen dökülür. ",
  "Nida varya seni çok SEVİYORUMMMM!!!!",
  "Ben senden önce hiçbir şey değildim, seninle birlikte her şey oldum. Sen benim Herşeyimsin",
]

// Nida Sözlüğü - yılbaşı temalı güncellendi
const nidaDictionary = ref([
  {
    word: "Nida:",
    definition: "Hem kızgın hem tatlı hem komik hem zeki hem mis gibi kokuyo, tam bir melek 🎄",
    isRevealed: false
  },
  {
    word: "Ödülün:",
    definition: "2024'ün en yaratıcı kurgu yazarı 🎭",
    isRevealed: false
  },
  {
    word: "Veysel!",
    definition: "Boku yedin 💩",
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

const revealDefinition = (index: number) => {
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
  @apply px-6 py-3 bg-gradient-to-r from-red-600 to-green-600 text-white rounded-full font-semibold transform transition-all duration-300 hover:scale-105 hover:shadow-lg active:scale-95 focus:outline-none;
}

.card {
  @apply backdrop-blur-sm rounded-xl shadow-xl p-6 transition-all duration-300 hover:shadow-2xl hover:scale-[1.02];
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