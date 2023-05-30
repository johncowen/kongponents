<template>
  <li
    :class="{
      active: props.selected
    }"
  >
    <KAction
      v-bind="attrs"
      :to="props.to"
    >
      <slot name="default" />
    </KAction>
  </li>
</template>

<script lang="ts">
export default {
  inheritAttrs: false,
}

</script>

<script setup lang="ts">
import KAction from '../KAction/KAction.vue'
import { useAttrs } from 'vue'

const attrs = useAttrs()
const props = defineProps({
  selected: {
    type: Boolean,
    default: false,
  },
  to: {
    type: [Object, String],
    default: null,
  },
})
</script>

<style lang="scss" scoped>
@import '@/styles/variables';
@import '@/styles/functions';
li + li {
  margin-left: var(--spacing-xs, spacing(xs));
}
li:not(:last-of-type) {
  margin-right: var(--spacing-xs, spacing(xs));
}
li > * {
  display: inline-block;
  padding: var(--spacing-md, spacing(md));
  padding-bottom: calc(var(--spacing-md, spacing(md)) - 4px);
  border-bottom: 4px solid transparent;
}
li.active > *,
li > *:hover {
  border-bottom-color: var(--KTabBottomBorderColor, var(--teal-300, color(teal-300)));
}

</style>
