<template>
  <nav
    aria-label="Secondary"
  >
    <KTabs>
      <KTabButton
        :selected="router.currentRoute.value.name === 'tabs'"
        :to="{
          name: 'tabs'
        }"
      >
        Alert Tab
      </KTabButton>
      <KTabButton
        :to="{
          name: 'alert'
        }"
      >
        Second Alert Tab
      </KTabButton>
      <KTabButton
        @click="click"
      >
        Button Tab
      </KTabButton>
    </KTabs>
  </nav>
</template>

<script setup lang="ts">
import { KTabs, KTabButton } from '@/components'
import { useRouter } from 'vue-router'
const router = useRouter()
const click = (e) => {
  alert('click')
}
</script>
