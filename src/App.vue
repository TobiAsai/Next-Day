<script lang="ts" setup>
import { onMounted, onUnmounted, ref } from 'vue'
import dayjs from 'dayjs'

const today = ref(dayjs().format("YYYY-MM-DD HH:mm:ss"))
const selectedDate = ref(dayjs().format('YYYY-MM-DD'))
const nextDay = ref('')
let timer: any = null

const animate = ref(false)

const calculateNextDay = () => {
  nextDay.value = dayjs(selectedDate.value).add(1, 'day').format('YYYY-MM-DD')
}

const useSystemDate = () => {
  selectedDate.value = dayjs().format('YYYY-MM-DD')
  calculateNextDay()
}

const updateToday = () => {
  today.value = dayjs().format("YYYY-MM-DD HH:mm:ss")
  animate.value = false
  requestAnimationFrame(() => {
    animate.value = true
  })
}

onMounted(() => {
  updateToday()
  timer = setInterval(updateToday, 1000)
})

onUnmounted(() => {
  clearInterval(timer)
})

// 預設計算
calculateNextDay()
</script>

<template>
  <div class="container time-container">
    <h1>第二天計算器</h1>
    <div :class="['time-text', { animate }]">今日日期： {{ today }}</div>
    
    <label>選擇日期：</label>
    <input type="date" v-model="selectedDate" @change="calculateNextDay" />

    <button @click="useSystemDate">使用當前系統時間</button>

    <h2>選擇的日期：{{ selectedDate }}</h2>

    <h2>下一天日期：{{ nextDay }}</h2>
  </div>
</template>

<style lang="scss" scoped >
.container {
  max-width: 400px;
  margin: auto;
  text-align: center;
}

input, button {
  margin: 10px;
  padding: 8px;
}

/* 背景漸變色 */
.time-container {
  // display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #b0c6ff, #96d7f2);
  position: relative;
  overflow: hidden;
}

/* 時間文字樣式 */
.time-text {
  font-size: 2rem;
  font-family: 'Cursive', sans-serif;
  font-weight: bold;
  color: #a1f0ff;
  text-shadow: 0 0 5px #a1f0ff, 0 0 15px #82d7f9;
  animation: glow-pulse 2s infinite ease-in-out;
  position: relative;
}

/* 漸變光暈 */
@keyframes glow-pulse {
  0%, 100% { transform: scale(1); opacity: 1; }
  50% { transform: scale(1.1); opacity: 0.8; }
}

/* 額外的星星粒子動畫 */
.time-container::before {
  content: '✨';
  font-size: 3rem;
  color: #ffcdff;
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(-50%);
  animation: twinkle 3s infinite ease-in-out;
}

@keyframes twinkle {
  0% { opacity: 0.5; transform: translateX(-50%) translateY(0); }
  50% { opacity: 1; transform: translateX(-50%) translateY(-10px); }
  100% { opacity: 0.5; transform: translateX(-50%) translateY(0); }
}

/* 讓動畫加強效果，時間文字隨著動畫微微漂浮 */
.animate {
  transform: scale(1.1) rotate(5deg);
}

</style>
