<script lang="ts" setup>
import { ref } from 'vue'
import dayjs from 'dayjs'

const selectedDate = ref(dayjs().format('YYYY-MM-DD'))
const nextDay = ref('')

const calculateNextDay = () => {
  nextDay.value = dayjs(selectedDate.value).add(1, 'day').format('YYYY-MM-DD')
}

const useSystemDate = () => {
  selectedDate.value = dayjs().format('YYYY-MM-DD')
  calculateNextDay()
}

// 預設計算
calculateNextDay()
</script>

<template>
  <div class="container">
    <h1>第二天計算器</h1>

    <label>選擇日期：</label>
    <input type="date" v-model="selectedDate" @change="calculateNextDay" />

    <button @click="useSystemDate">使用當前系統時間</button>

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
</style>
