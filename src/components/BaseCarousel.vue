<template>
  <div class="BaseCarousel">
    <div class="relative">
      <div ref="$slideTrack" class="relative w-full m-auto">
        <ul class="flex" :style="slideTrackStyle()" @transitionend.self="getTransitionEnd">
          <li v-for="(item, index) in extendData" :key="item.slideId" class="flex items-center justify-center"
            style="flex: calc(220px) 0 0" @mouseup="transitionEnd = false" :data-id="index" @click="jumpSlideActiveId(index)">
            <slot name="slideItem" :slideData="data[item.slideObj]"></slot>
          </li>
        </ul>
      </div>

      <div class="flex items-center justify-center z-50 mx-auto my-12 drop-shadow hover:drop-shadow-xl transition-all" v-show="!choiceId">
        <div @click="slideDirection(-1)">
          <slot name="arrowLeft"></slot>
        </div>
        <div @click="slideDirection(1)">
          <slot name="arrowRight"></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import { useWindowSize } from '@vueuse/core'

const props = defineProps({
  slidePending: {
    type: Number,
    default: 0
  },
  displayAmount: {
    type: Number,
    default: 2
  },
  elementWidth: {
    type: Object,
    default: () => {
      return {
        pc: '1013px',
        mob: '320px'
      }
    }
  },
  focus: {
    type: String,
    default: 'center'
  },
  needPagination: {
    type: Boolean,
    default: false
  },
  data: {
    type: Array,
    default: () => {
      return [
        {
          id: 1,
          text: 'slide1'
        },
        {
          id: 2,
          text: 'slide2'
        },
        {
          id: 3,
          text: 'slide3'
        },
        {
          id: 4,
          text: 'slide4'
        },
        {
          id: 5,
          text: 'slide5'
        }
      ]
    }
  },
  mediaSize: {
    type: Number,
    default: 1024
  },
  autoPlay: {
    type: Boolean,
    default: true
  },
  choiceId: {
    type: [Number],
    default: null
  },
})

const { width } = useWindowSize()
const storeWindowWidth = computed(() => {
  return width
})
const extendData = computed(() => {
  const list = []
  for (let i = 0; i < props.data.length * 3; i++) {
    list.push({
      slideId: i,
      slideObj: i % props.data.length
    })
  }
  return list
})

const align = computed(() => {
  if (props.focus === 'left') return 0
  if (props.focus === 'center') return Math.floor(props.displayAmount / 2)
  return props.displayAmount
})
const slideActiveId = ref(props.data.length * 3 / 2)

const distanceOffset = computed(() => {
  if (storeWindowWidth.value < props.mediaSize) {
    return props.elementWidth.mob
  }

  return props.elementWidth.pc
})
const slideTrackStyle = () => {
  console.log('1',align.value, slideActiveId.value, transitionEnd.value)
  
  const distance = align.value - 3 - (slideActiveId.value)
  if (transitionEnd.value && isLimited.value) {
    return {
      transform: 'translateX(calc(' + distanceOffset.value + ' * ' + distance + ' + (100% - '+distanceOffset.value+') / 2 ))'
    }
  }
  return {
    transform: 'translateX(calc('+ distanceOffset.value + ' * ' + distance + ' + (100% - '+distanceOffset.value+') / 2 ))',
    transition: 'transform .3s'
  }
}

const transitionEnd = ref(false)
const isLimited = ref(false)
const slideRange = {
  max: props.data.length * 2,
  min: props.data.length - 1
}
const slideDirection = (direction) => {
  if (slideActiveId.value >= slideRange.max || slideActiveId.value <= slideRange.min) {
    return
  }
  transitionEnd.value = false
  slideActiveId.value = slideActiveId.value + direction
  isLimited.value = false
}

const getTransitionEnd = () => {
  transitionEnd.value = true
  if (slideActiveId.value >= slideRange.max) {
    isLimited.value = true
    slideActiveId.value = props.data.length
    return
  }
  if (slideActiveId.value <= slideRange.min) {
    isLimited.value = true
    slideActiveId.value = props.data.length * 2 - 1
  }
}

const jumpSlideActiveId = (id) => {
  slideActiveId.value = id
}

</script>

<style lang="scss">
.BaseCarousel {
}

// xxl | xl | lg | md | sm
// @include screen (xl) {}
</style>
