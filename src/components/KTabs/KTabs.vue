<template>
  <div class="k-tabs">
    <template
      v-if="slots.default"
    >
      <ul
        v-bind="$attrs"
      >
        <slot
          name="default"
        />
      </ul>
    </template>
    <template
      v-else
    >
      <ul
        aria-label="Tabs"
        role="tablist"
      >
        <KTabButton
          v-for="(tab, i) in tabs"
          :key="tab.hash"
          :aria-controls="`panel-${i}`"
          :aria-selected="activeTab === tab.hash ? 'true' : 'false'"
          class="tab-item"
          :class="{ active: activeTab === tab.hash }"
          role="tab"
          tabindex="0"
          @click="handleTabChange(tab.hash)"
          @keydown.enter.prevent="handleTabChange(tab.hash)"
          @keydown.space.prevent="handleTabChange(tab.hash)"
        >
          <slot :name="`${tab.hash.replace('#','')}-anchor`">
            {{ tab.title }}
          </slot>
        </KTabButton>
      </ul>
    </template>

    <div
      v-for="(tab, i) in tabs"
      :id="`panel-${i}`"
      :key="tab.hash"
      :aria-labelledby="`${tab.hash.replace('#','')}-tab`"
      class="tab-container"
      role="tabpanel"
      tabindex="0"
    >
      <slot
        v-if="activeTab === tab.hash"
        :name="tab.hash.replace('#','')"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, watch, PropType, useSlots } from 'vue'
import KTabButton from '../KTabButton/KTabButton.vue'
import type { Tab } from '@/types'
const slots = useSlots()

const props = defineProps({
  /**
     * Array of Tab objects [{hash: '#tab1', title: 'tab1'}, {hash: '#tab2', title: 'tab2'}]
     */
  tabs: {
    type: Array as PropType<Tab[]>,
    required: false,
    default: () => [],
  },
  /**
     * A set tab hash to use as default
     */
  modelValue: {
    type: String,
    default: '',
    validator: (val: string): boolean => val === '' || (val.includes('#') && !val.includes(' ')),
  },
})

const emit = defineEmits<{
  (e: 'update:modelValue', val: string): void
  (e: 'changed', val: string): void
}>()

const activeTab = ref(props.modelValue ? props.modelValue : props.tabs[0]?.hash)

const handleTabChange = (tab: string): void => {
  activeTab.value = tab
  emit('changed', tab)
  emit('update:modelValue', tab)
}

watch(() => props.modelValue, (newTabHash) => {
  activeTab.value = newTabHash
  emit('changed', newTabHash)
  emit('update:modelValue', newTabHash)
})
</script>

<style lang="scss" scoped>
@import '@/styles/variables';
@import '@/styles/functions';

.k-tabs {
  ul {
    border-bottom: 1px solid var(--KTabsBottomBorderColor, var(--grey-300, color(grey-300)));
    display: flex;
    font-size: 18px;
    line-height: 20px;
    list-style: none;
    margin-bottom: 0;
    padding-left: 0;

  }
}
</style>
