<script setup lang="ts">
import SearchInput from '@/components/SearchInput.vue'
import { Button } from '@/components/shadcn/ui/button'
import { RouterLink, RouterView } from 'vue-router'

const activeClass = '!text-black'
const inactiveClass = 'text-gray-400'
</script>

<template>
  <nav class="w-full hfit bg-gray-100 flex p-2 justify-between items-center">
    <h1 class="text-lg font-semibold">Programacion Front End</h1>
    <div class="flex gap-4 items-center">
      <router-link
        :exact-active-class="activeClass"
        :class="[inactiveClass]"
        exact
        :to="{ name: 'grades' }"
        >Calculo de calificaciones
      </router-link>
      <router-link
        :exact-active-class="activeClass"
        :class="[inactiveClass]"
        exact
        :to="{ name: 'register' }"
        >Formulario de Registro
      </router-link>
      <span :class="[inactiveClass, 'cursor-not-allowed']">En construccion</span>
    </div>
    <div class="flex gap-2 items-center">
      <search-input></search-input>
      <Button variant="outline" class="hover:border-black text-green-600 border-green-600">Busqueda</Button>
    </div>
  </nav>
  <RouterView />
</template>

<style scoped></style>
