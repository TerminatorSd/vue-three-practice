<template>
  <slot></slot>
  <div class="rate" @mouseout="mouseOut" :style="fontStyle">
    <span @mouseover="mouseOver(num)" v-for="num in 5" :key="num">☆</span>
    <span class="hollow" :style="fontWidth">
      <span @mouseover="mouseOver(num)" @click="onRate(num)" v-for="num in 5" :key="num">★</span>
    </span>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, computed, ref } from 'vue';
const themeObj = {
  'black': '#00',
  'white': '#fff',
  'red': '#f5222d',
  'orange': '#fa541c',
  'yellow': '#fadb14',
  'green': '#73d13d',
  'blue': '#40a9ff',
}
let props = defineProps({
  modelValue: Number,
  theme: {
    type: String,
    default: 'orange'
  }
})
const fontStyle = computed(() => {
  return `color: ${themeObj[props.theme]}`;
})

let width = ref(props.modelValue);
const fontWidth = computed(() => {
  return `width: ${width.value}em;`
})
const mouseOut = () => {
  width.value = props.modelValue;
}
const mouseOver = (num) => {
  width.value = num;
}

let emits = defineEmits(['update:modelValue']);
const onRate = (num) => {
  emits('update:modelValue', num)
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.rate {
  position: relative;
  display: inline-block;
}

.rate>.hollow {
  position: absolute;
  display: inline-block;
  top: 0;
  left: 0;
  width: 0;
  overflow: hidden;
}
</style>
