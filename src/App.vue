<!-- src/App.vue -->
<script setup lang="ts">
import { ref } from 'vue'
import Calendar from './components/Calendar.vue'
import SelectControl from './components/SelectControl.vue'

interface Option {
  value: string,
  description: string
}

const lang = ref('ru')
const langOptions: Option[] = [
  { value: 'ru', description: 'Русский' },
  { value: 'en', description: 'Английский' },
]
const selected = ref('')
const onSelectDate = (date: string) => {
  selected.value = date
  alert(`Выбранная дата: ${date}`)
}
const changeLang = (value: string) => {
  lang.value = value
}
</script>

<template>
  <Calendar :locale="lang" @select="onSelectDate" />
  <SelectControl :lang="lang" :options="langOptions" @changeLang="changeLang" />
</template>

