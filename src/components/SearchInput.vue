<script setup lang="ts">
import { Search } from 'lucide-vue-next'

const search = defineModel<string | undefined>({
  default: undefined,
})
</script>

<template>
  <div class="relative w-fit flex gap-4">
    <label for="default-search" class="mb-2 text-sm font-medium text-gray-900 sr-only"
      >Search</label
    >
    <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
      <Search color="gray" :size="16" />
    </div>
    <input
      type="search"
      id="default-search"
      v-model="search"
      class="block w-full p-2.5 ps-8 text-sm text-gray-900 border rounded-2xl focus:ring-blue-500 focus:border-blue-500"
      placeholder="Search"
    />
  </div>
</template>

<style scoped></style>
