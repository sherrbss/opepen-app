<template>
  <article class="image" :class="{ loaded }" v-intersection-observer="loadImage">
    <div class="inner image">
      <img
        v-if="uri"
        ref="imageEl"
        :src="uri"
        @error="loadOriginal"
        @load="imageLoaded"
      >
    </div>
  </article>
</template>

<script setup>
import { vIntersectionObserver } from '@vueuse/components'
import { imageURI } from '~/helpers/images'

const props = defineProps({
  image: Object,
  version: String,
})

const uri = ref('')
const loaded = ref(false)

const loadImage = ([{ isIntersecting }]) => {
  if (! isIntersecting) return

  const version = props.image.versions[props.version] ? props.version : ''

  uri.value = imageURI(props.image, version)
}
const loadOriginal = () => {
  console.log('error')
  uri.value = imageURI(props.image)
}

// Image loaded event
const imageLoaded = () => loaded.value = true
</script>

<style lang="postcss">
article.image {
  overflow: hidden;
  background-color: var(--gray-z-3);
  border: 1px solid var(--gray-z-4);
  border-radius: var(--size-5);
  border-top-left-radius: var(--size-1);

  position: relative;
  height: 0;
  padding-bottom: 100%;

  .inner {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .image {
    img {
      position: absolute;
      top: 0;
      left: 0;
    }
  }

  img {
    transform: scale(1.2);
    width: 100%;
    opacity: 0;
    transition: all var(--speed);
  }

  &.loaded {
    img {
      transform: scale(1.025);
      opacity: 1;
    }
  }

  &.appear {
    opacity: 0.5;
    transition: all var(--speed-slow);

    img {
      opacity: 0.001;
    }

    &.up {
      transform: translateY(var(--size-6));
    }

    &.loaded {
      opacity: 1;
      transform: translateY(0);

      img {
        opacity: 1;
      }
    }
  }
}
</style>
