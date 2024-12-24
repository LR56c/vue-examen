<script setup lang="ts">
import { computed, reactive } from 'vue'
import { Button } from '@/components/shadcn/ui/button'
import { toTypedSchema } from '@vee-validate/zod'
import { Field, useForm } from 'vee-validate'
import { z } from 'zod'

const validationSchema = toTypedSchema(
  z.object({
    nota1: z.number().min(10).max(70),
    nota2: z.number().min(10).max(70),
    nota3: z.number().min(10).max(70),
    asistencia: z.number().min(0).max(100),
  }),
)
const form = useForm({
  validationSchema,
})

const notaPromedio = reactive({
  promedio: 0,
  aprobado: false,
  init: false,
})

const onSubmit = form.handleSubmit((values) => {
  const promedio = (values.nota1 * 0.35 + values.nota2 * 0.35 + values.nota3 * 0.3)
  const aprobado = promedio >= 40 && values.asistencia >= 80
  notaPromedio.promedio = promedio
  notaPromedio.aprobado = aprobado
  notaPromedio.init = true
  form.resetForm()
})

const hasErrors = computed(() => Object.keys(form.errors.value).length > 0)
</script>

<template>
  <div class="flex flex-col items-center justify-center w-full h-screen">
    <form @submit.prevent="onSubmit" class="flex flex-col gap-4 w-full max-w-xl">
      <Field v-slot="{ field }" name="nota1" as="div" class="grid w-full items-center gap-1.5">
        <Label for="nota1">Nota 1</Label>
        <Input v-bind="field" required min="10" max="70" id="nota1" type="number" />
      </Field>
      <Field v-slot="{ field }" name="nota2" as="div" class="grid w-full items-center gap-1.5">
        <Label for="nota2">Nota 2</Label>
        <Input v-bind="field" required min="10" max="70" id="nota2" type="number" />
      </Field>
      <Field v-slot="{ field }" name="nota3" as="div" class="grid w-full items-center gap-1.5">
        <Label for="nota3">Nota 3</Label>
        <Input v-bind="field" required min="10" max="70" id="nota3" type="number" />
      </Field>
      <Field v-slot="{ field }" name="asistencia" as="div" class="grid w-full items-center gap-1.5">
        <Label for="asistencia">Asistencia %</Label>
        <Input v-bind="field" required min="0" max="100" id="asistencia" type="number" />
      </Field>
      <Button type="submit" variant="default" class="w-fit bg-blue-600 text-white px-8"
        >Calcular
      </Button>
      <p v-if="hasErrors" class="text-red-500">
        Por favor, ingrese valores validos para las notas y la asistencia
      </p>
    </form>
    <div v-if="notaPromedio.init">
      <p class="text-xl">
        Promedio: {{ notaPromedio.promedio.toFixed(2) }}
      </p>
      <p class="text-xl">
        Estado: {{ notaPromedio.aprobado ? 'Aprobado' : 'Desaprobado' }}
      </p>
    </div>
  </div>
</template>
