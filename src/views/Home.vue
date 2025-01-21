<script setup lang="ts">
import Drawer from '@/components/Drawer.vue'
import JSONViewer from '@/components/JSONViewer.vue'
import { useRegle } from '@regle/core'
import { applyIf, minLength, required } from '@regle/rules'
import { ref } from 'vue'

const condition = ref(true)

const { r$ } = useRegle(
  { name: '' },
  {
    name: {
      required: applyIf(condition, required),
      minLength: applyIf(condition, minLength(3)),
    },
  },
)
</script>

<template>
  <div class="px-6 text-gray-900 antialiased">
    <div class="mx-auto max-w-xl py-12 md:max-w-4xl">
      <h2 class="text-2xl mb-8">Regle reproduction</h2>
      <div class="flex flex-col">
        <label>Your name</label>
        <input class="border p-2 rounded" v-model="r$.$value.name" placeholder="Victor Regle" />
        <ul class="text-red-400 text-sm mt-1" v-if="r$.$errors.name.length">
          <li v-for="error of r$.$errors.name" :key="error">{{ error }}</li>
        </ul>
      </div>
      <Drawer>
        <JSONViewer :data="r$" />
      </Drawer>
    </div>
  </div>
</template>
