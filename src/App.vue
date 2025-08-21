<!-- src/App.vue -->
<script setup lang="ts">
import { ref } from 'vue'
import Calendar from './components/Calendar.vue'
import SelectControl from './components/SelectControl.vue'

const lang = ref('ru')
const langOptions = [
  {value: 'ru', description: 'Русский'},
  {value: 'en', description: 'Английский'},
]
const selected = ref('')
const onSelectDate = (date) => {
  selected.value = date
  alert(`Выбранная дата: ${date}`)
}
const changeLang = (value: string) => {
  lang.value = value
}
</script>


<template>
  <div class="app">
    <div style="margin-top: 20px;">
      <SelectControl
        :lang="lang"
        :options="langOptions"
        @changeLang="changeLang"

      />
    </div>
    <Calendar 
      :locale="lang" 
      @select="onSelectDate" 
    />
  </div>
</template>