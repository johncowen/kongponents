<template>
  <router-link
    v-if="props.to"
    v-bind="$attrs"
    :to="props.to"
  >
    <slot name="default" />
  </router-link>
  <a
    v-else-if="$attrs.href"
    v-bind="$attrs"
  >
    <slot name="default" />
  </a>
  <button
    v-else
    v-bind="$attrs"
  >
    <slot name="default" />
  </button>
</template>

<script lang="ts" setup>
const props = defineProps({
  to: {
    type: [Object, String],
    default: null,
  },
})
</script>

<style lang="scss" scoped>
button {
  appearance: none;
  background: initial;
  border: initial;
}
a, button {
  cursor: pointer;
  font-size: inherit;
  font-weight: inherit;
  text-decoration: inherit;
  color: inherit;
  line-height: inherit;
}
</style>
