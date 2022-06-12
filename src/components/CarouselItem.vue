<template>
  <div class="SalesSlideItem w-full" >
    <article :key="data.icon" class="SalesSlideItem__slideItem">
      <div class="SalesSlideItem__img cursor-pointer transition-all duration-500 hover:z-30 hover:scale-120"
          :class="{'SalesSlideItem__choice z-30' : choiceId === data.id}"
          @click="choiceCard(data.id)"
      >
        <div class="relative overflow-hidden">
          <img class="SalesSlideItem__cover relative z-10 w-full rounded-lg" height="382" src="https://xinma.com.tw/assets/demo/cover.png" alt="">
          <div class="SalesSlideItem__inner absolute top-0 left-0 z-20 w-full border rounded-lg">
            <img class="w-full" :class="{'rotate-180' : cardRotate, 'opacity-100': choiceId, 'opacity-0': !choiceId }" :src="`https://xinma.com.tw/assets/demo/tarot${data.id}.jpg`" alt="">
          </div>
        </div>
        <div class="absolute top-0 left-0 w-full h-full z-10 flex items-center justify-center text-stone-50 opacity-0 hover:opacity-100 transition-opacity">
          <div class="bg-zinc-900 py-1.5 px-3 rounded-full">選這張</div>
        </div>
      </div>
    </article>
  </div>
</template>

<script setup>
import { computed } from 'vue';
const props = defineProps({
  data: {
    type: Object,
    default: () => {
      return {
        id: '',
      }
    }
  },
  choiceId: {
    type: [Number],
    default: null
  }
})

const emit = defineEmits(['choice'])
const choiceCard = (val) => {
  if(props.choiceId) return
  emit('choice', val)
}

const cardRotate = computed(() => {
  if(props.choiceId) return Math.floor(Math.random() * 4) % 2
  return
})
</script>

<style lang="scss">
.SalesSlideItem {
  
  &__img {
    &:hover {
      @apply z-30;
      transform: scale(1.2);
      
    }
  }
  &__choice {
    transform: scale(1.2);
    .SalesSlideItem__cover {
      transform: rotateY(180deg);
    }
    .SalesSlideItem__inner {
      transform: rotateY(360deg);
    }
  }
  &__cover {
    height: 382px;
    transform: rotateY(0deg);
    transition: transform .4s ease-in .2s;
  }
  &__inner {
    transform:  rotateY(180deg);
    backface-visibility: hidden;
    transition: transform .4s ease-in  .2s;
  }

  &__slideItem {
    position: relative;
    display: flex;
    flex: calc(100% - 58px) 0 0;
    flex-direction: column;
    justify-content: space-between;
  }

   @media screen and (max-width: 991px) {
    height: 100%;
  }
}

// xxl | xl | lg | md | sm
// @include screen (xl) {}
</style>
