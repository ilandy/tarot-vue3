<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { computed, ref } from 'vue';
import BaseCarousel from './components/BaseCarousel.vue'
import CarouselItem from './components/CarouselItem.vue'

const cardNumber = 78
const choiceId = ref(NaN)

const tarotList = computed(() => {
  const list = []
  for(let card = 1; card <= cardNumber; card++) {
    list.push({ id: card })
  } 
  return list
})

const baseRandom = (lower, upper) => {
  return lower + Math.floor(Math.random() * (upper - lower + 1));
}

const shuffleSelf = (array, size) =>  {
      var index = -1,
          length = array.length,
          lastIndex = length - 1;

      size = size === undefined ? length : size;
      while (++index < size) {
        var rand = baseRandom(index, lastIndex),
            value = array[rand];

        array[rand] = array[index];
        array[index] = value;
      }
      array.length = size;
      return array;
}

const randomTrotList = computed(() => {
  return shuffleSelf(tarotList.value)
})

const getChoice = (val) => {
  choiceId.value = val
}

const getSlideActiveId = (val) => {
  SlideActiveId
}

</script>

<template>
<div class="Tarot">
  <BaseCarousel
    :class="{'pointer-events-none' : choiceId}"
    :displayAmount="6"
    :element-width="{
        pc: '220px',
        mob: '220px'
      }"
    :data="randomTrotList"
    :choiceId="choiceId"
    @slideActiveId="getSlideActiveId"
    
  >
    <template v-slot:slideItem="props">
      <CarouselItem 
        :data="props.slideData"
        @choice="getChoice"
        :choiceId="choiceId"
      ></CarouselItem>
    </template>

    <template v-slot:arrowLeft>
      <div class="bg-zinc-800 cursor-pointer transition-all text-white rounded-l-full py-2 pl-6 pr-4 border-r border-gray-600 hover:bg-slate-900">
        向左邊移動
      </div>
    </template>
    <template v-slot:arrowRight>
      <div class="bg-zinc-800 cursor-pointer transition-all text-white rounded-r-full py-2 pr-6 pl-4 hover:bg-slate-900">
        向右邊移動
      </div>
    </template>
  </BaseCarousel>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  overflow: hidden;
  padding-top: 90px;
  padding-bottom: 90px;
  margin-top: -20px;
  margin-bottom: -60px;
}
</style>
