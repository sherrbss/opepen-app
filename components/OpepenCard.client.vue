<template>
<article class="opepen-card" :style="wrapperStyle">
  <div @click="onClick">
    <Image v-if="token.image" :image="token.image" version="sm" />
    <img v-else :src="`https://opepen.nyc3.cdn.digitaloceanspaces.com/images/rare/${set}.svg`" :alt="`Rare 1/${set} Opepen Preview ${id}`">

    <div class="text">
      <h1>Opepen {{ id }}</h1>
      <slot name="subline">
        <p>Edition of {{ set }}</p>
      </slot>
    </div>
  </div>
</article>
</template>

<script setup>
import { getRandomArbitrary } from '~/helpers/random'
import { delay } from '~/helpers/time'

const {
  token,
  set,
} = defineProps({
  token: Object,
  set: [Number, String],
})

const id = token.token_id

// ==============================================================
// Style & Interaction
// ==============================================================
const rotation = ref(getRandomArbitrary(-12, 12))
const scale = ref(1)
const wrapperStyle = computed(() => ({
  transform: `rotate(${rotation.value}deg) scale(${scale.value})`,
}))

const wiggle = async () => {
  rotation.value += getRandomArbitrary(5, 10)
  scale.value = 1.05
  await delay(100)
  rotation.value -= getRandomArbitrary(10, 20)
  await delay(80)
  rotation.value += getRandomArbitrary(5, 10)
  await delay(50)
  scale.value = 1
  rotation.value = getRandomArbitrary(-5, 5)
}
const onClick = async () => {
  wiggle()
  await delay(150)
  window.open(`https://opensea.io/assets/ethereum/0x6339e5e072086621540d0362c4e3cea0d643e114/${id}`, '_blank')
}
</script>

<style lang="postcss" scoped>
.opepen-card {
  position: relative;
  width: 100%;
  height: 0;
  padding-bottom: 125%;
  transition: all var(--speed-fast);
  transform-origin: center;

  /* Inner */
  > div {
    container-type: inline-size;
    cursor: pointer;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--gray-z-1);
    border: var(--border);
    border-radius: var(--size-4);
    border-top-left-radius: var(--size-0);
    box-shadow: 0 var(--size-2) var(--size-5) 0 var(--semi-shade);
    display: grid;
    grid-template-rows: 4fr 1fr;

    .image {
      border: none !important;
      border-radius: 0;
      border-top-right-radius: var(--size-4);
      border-top-left-radius: var(--size-0);
      border-bottom: var(--border);
    }

    img {
      border-top-right-radius: var(--size-4);
      border-top-left-radius: var(--size-0);
      border-bottom: var(--border);
    }

    .text {
      padding: 0 var(--size-4);
      display: flex;
      flex-direction: column;
      justify-content: center;
      text-transform: uppercase;
      font-weight: var(--font-weight-bold);
      letter-spacing: var(--letter-spacing-md);
      user-select: none;
      line-height: 1;
      width: 100%;
      overflow: hidden;

      h1 {
        font-size: 6cqw;
        margin-bottom: 2cqw;
      }

      :deep(p) {
        font-size: 4.5cqw;
        color: var(--gray-z-7);
        max-width: 100%;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    }
  }
}
</style>
