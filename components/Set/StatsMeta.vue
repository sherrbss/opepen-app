<template>
  <aside v-if="set.name" class="stats-meta">
    <p>Opepen holders decide if a set is released: <span><span class="nowrap">If demand is</span> less than {{ set.min_subscription_percentage }}% within one of the brackets, <span class="nowrap">the set</span> will be replaced.</span></p>
  </aside>
</template>

<script setup>
defineProps({
  set: Object,
})
</script>

<style lang="postcss" scoped>
  .stats-meta {
    font-weight: var(--font-weight-bold);
    font-size: var(--font-sm);
    line-height: var(--line-height-md);

    margin-top: calc(-1 * var(--size-4));

    @media (--md) {
      margin-top: calc(-1 * var(--size-6));
    }

    p {
      span {
        color: var(--gray-z-6);
      }
    }
  }
</style>
