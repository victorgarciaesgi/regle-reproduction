<template>
  <andypf-json-viewer ref="json" :data="JSON.stringify(props.data)"></andypf-json-viewer>
</template>

<script setup lang="ts">
import { useTemplateRef, watch } from 'vue'

const json = useTemplateRef<{ data: unknown }>('json')

const props = defineProps<{
  data: unknown
}>()

watch(
  () => props.data,
  (value) => {
    if (json.value) {
      json.value.data = JSON.stringify(value)
    }
  },
  { deep: true },
)
</script>

<style lang="scss" scoped></style>
