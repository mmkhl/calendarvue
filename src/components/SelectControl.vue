<script setup lang="ts">
  import { defineProps, defineEmits } from 'vue';
  const emit = defineEmits(['changeLang'])
  const props = defineProps(['options'])

</script>

<template>
  <label class="select-control">
    Язык: 
    <select id="options-list" @change="(event) => emit('changeLang', event.target?.value )">
      <template v-for="opt in props.options" :key="opt.value">
        <option :value="opt?.value">{{opt?.description}}</option>
      </template>
    </select>
  </label>
</template>


<style lang="scss" scoped>
.select-control{
  display: inline-block;
  margin: 20px 0 0 0;
}
</style>