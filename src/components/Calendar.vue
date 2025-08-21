<script setup lang="ts">
import { ref, computed, watch, defineProps, defineEmits } from 'vue'

const emit = defineEmits(['select'])
const props = defineProps({
  date: {
    type: String,
    default: null
  },
  locale: {
    type: String,
    default: 'ru'
  }
})


const selectedDate = ref()

const currentDate = ref(new Date())

if (props.date) {
  const [year, month, day] = props.date.split('-').map(Number)
  currentDate.value = new Date(year, month - 1, day)
  selectedDate.value = new Date(year, month - 1, day)
} else {
  const today = new Date()
  selectedDate.value = new Date(today.getFullYear(), today.getMonth(), today.getDate())
}

const months: { [key: string]: any } = {
  ru: [ 
    'Январь',
    'Февраль',
    'Март',
    'Апрель',
    'Май',
    'Июнь',
    'Июль',
    'Август',
    'Сентябрь',
    'Октябрь',
    'Ноябрь',
    'Декабрь'
  ],
  en: [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December'
  ]
}



const weekDays: { [key: string]: any } = {
  ru: [
    'Пн', 
    'Вт', 
    'Ср', 
    'Чт', 
    'Пт', 
    'Сб', 
    'Вс'
  ],
  en: [
    'Mon', 
    'Tue', 
    'Wed', 
    'Thu', 
    'Fri', 
    'Sat', 
    'Sun'
  ]
}

const currentMonthLabel = computed(() => {
  const monthIndex = currentDate.value.getMonth()
  return `${months[props.locale][monthIndex]} ${currentDate.value.getFullYear()}`
})

const weekdays = computed(() => {
  return weekDays[props.locale]
})

const daysInMonth = computed(() => {
  const year = currentDate.value.getFullYear()
  const month = currentDate.value.getMonth()
  const firstDay = new Date(year, month, 1).getDay()
  const lastDate = new Date(year, month + 1, 0).getDate()

  const days = []

  const offset = firstDay === 0 ? 6 : firstDay - 1

  for (let i = offset; i > 0; i--) {
    days.push({

    })
  }

  const today = new Date()
  const todayFormatted = new Date(today.getFullYear(), today.getMonth(), today.getDate())

  for (let day = 1; day <= lastDate; day++) {
    const date = new Date(year, month, day)
    const dateFormatted = new Date(date.getFullYear(), date.getMonth(), date.getDate())
    const isSelected = selectedDate.value && dateFormatted.getTime() === selectedDate.value.getTime()
    const isToday = dateFormatted.getTime() === todayFormatted.getTime()

    days.push({
      number: day,
      date,
      isToday,
      isSelected
    })
  }
  return days
})

const prevMonth = () => {
  currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() - 1, 1)
}

const nextMonth = () => {
  currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() + 1, 1)
}

const selectDay = (day) => {
  const clickedDate = new Date(day.date.getFullYear(), day.date.getMonth(), day.date.getDate())
  selectedDate.value = clickedDate

  const formattedDate = `${clickedDate.getFullYear()}-${String(clickedDate.getMonth() + 1).padStart(2, '0')}-${String(clickedDate.getDate()).padStart(2, '0')}`
  emit('select', formattedDate)
}

watch(() => props.date, (newVal) => {
  if (!newVal) {
    return;
  }
  const [year, month, day] = newVal.split('-').map(Number)
  const newDate = new Date(year, month - 1, day)
  selectedDate.value = newDate
  currentDate.value = new Date(year, month - 1, 1)
})
</script>

<template>
  <div class="calendar">
    <div class="calendar-header">
      <button @click="prevMonth" class="nav-btn">&#9668;</button>
      <h4>{{ currentMonthLabel }}</h4>
      <button @click="nextMonth" class="nav-btn"> &#9658; </button>
    </div>

    <div class="weekdays">
      <div v-for="day in weekdays" :key="day" class="weekday">
        {{ day }}
      </div>
    </div>

    <div class="days">
      <div v-for="day, index in daysInMonth" :key="index"
        :class="['day', { 'today': day.isToday, 'selected': day.isSelected, 'active': day.hasOwnProperty('number') }]"
        @click="selectDay(day)">
        {{ day.number }}
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.calendar {
  width: fit-content;
  font-family: Arial, sans-serif;
  border: 1px solid #d6d6d6;

  &-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2px;

    h4 {
      margin: 5px 0;
      font-weight: 500;
    }
  }

  .nav-btn {
    background: none;
    border: none;
    cursor: pointer;
  }

  .weekdays {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    padding: 8px 0 15px 0;
    text-align: center;
    font-size: 0.9em;
  }

  .weekday {
    padding: 5px;
  }

  .days {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
  }

  .day {
    padding: 10px 12px;
    text-align: center;
    background-color: white;
    border: 1px solid white;

    &.active {
      cursor: pointer;

    }

    &.active:hover:not(.selected){
      border: 1px solid #a3e1e9;
    }

    &.today {
      background-color: #807d7d;
      color: white;
      font-weight: bold;
    }

    &.selected {
      border: 1px solid #71cbe5;
    }
  }
}
</style>