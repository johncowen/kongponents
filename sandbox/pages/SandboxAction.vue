<template>
  <KAction
    @click="click"
  >
    Button with click
  </KAction>
  <KAction
    :to="{
      name: 'action'
    }"
  >
    Link with href
  </KAction>
</template>

<script setup lang="ts">
import { KAction } from '@/components'
const click = () => {
  alert('Button clicked')
}
</script>
