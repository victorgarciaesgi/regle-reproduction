<script setup lang="ts">
import Drawer from '@/components/Drawer.vue'
import JSONViewer from '@/components/JSONViewer.vue'
import { useRegle } from '@regle/core'
import { required } from '@regle/rules'
import { ref } from 'vue'

const INITIAL_DATA = {
  field1: '',
  field2: '',
};

const form = ref({
    tab: structuredClone(INITIAL_DATA),
    tabs: Array.from({ length: 4 }, () => structuredClone(INITIAL_DATA)),
});

const { r$ } = useRegle(
  form,
  {
    tab: {
      field1: { required },
      field2: { required },
    },
    tabs: {
        $each: {
          field1: { required },
          field2: { required },
        },
    },
  },
)

const submit = async () => {
    const { result: isValid } = await r$.$validate();
    if (isValid) {
      console.log('submit');
    }
};
</script>

<template>
  <div class="px-6 text-gray-900 antialiased">
    <div class="mx-auto max-w-xl py-12 md:max-w-4xl">
      <h2 class="text-2xl mb-8">Regle reproduction</h2>
      <div class="flex flex-col">
          <div>
              <h2>Tab 1: {{ r$.$fields.tab.$error }}</h2>
              <div class="grid grid-cols-2 items-start">
                  <label class="flex flex-col rounded-lg bg-box p-8">
                      field 1
                      <input class="border p-2 rounded" v-model="form.tab.field1" />
                      <ul class="text-red-400 text-sm mt-1" v-if="r$.$errors.tab.field1.length">
                        <li v-for="error of r$.$errors.tab.field1" :key="error">{{ error }}</li>
                      </ul>
                  </label>
                  <label class="flex flex-col rounded-lg bg-box p-8">
                      field 2
                      <input class="border p-2 rounded" v-model="form.tab.field2" />
                      <ul class="text-red-400 text-sm mt-1" v-if="r$.$errors.tab.field2.length">
                        <li v-for="error of r$.$errors.tab.field2" :key="error">{{ error }}</li>
                      </ul>
                  </label>
              </div>
          </div>
          <div
              v-for="(tab, i) in form.tabs"
              :key="i"
          >
              <h2>Tab {{ i+2 }}: {{ r$.$fields.tabs.$each[i]?.$error }}</h2>
              <div class="grid grid-cols-2 items-start">
                <label class="flex flex-col rounded-lg bg-box p-8">
                      field 1
                      <input class="border p-2 rounded" v-model="tab.field1" />
                      <ul class="text-red-400 text-sm mt-1" v-if="r$.$errors.tabs.$each[i].field1.length">
                        <li v-for="error of r$.$errors.tabs.$each[i].field1" :key="error">{{ error }}</li>
                      </ul>
                  </label>
                  <label class="flex flex-col rounded-lg bg-box p-8">
                      field 2
                      <input class="border p-2 rounded" v-model="tab.field2" />
                      <ul class="text-red-400 text-sm mt-1" v-if="r$.$errors.tabs.$each[i].field2.length">
                        <li v-for="error of r$.$errors.tabs.$each[i].field2" :key="error">{{ error }}</li>
                      </ul>
                  </label>
              </div>
          </div>
      </div>
      <Drawer>
        <JSONViewer :data="r$" />
      </Drawer>
    </div>
  </div>
</template>
