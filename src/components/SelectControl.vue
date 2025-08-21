<script setup lang="ts">
  import { defineProps, defineEmits } from 'vue';
  const emit = defineEmits(['changeLang'])
  const props = defineProps({
    lang: {
      type: String,
      default: 'ru',
    },
    options: {
      type: Array,
      default: []
    }
  })

</script>

<template>
  <label>
    Язык: 
    <select @change="(event) => (emit('changeLang', event.target.value ))">
      <template v-for="opt in options" :key="opt.value">
        <option :value="opt.value">{{opt.description}}</option>
      </template>
    </select>
  </label>
</template>



<style scoped>

</style>