<script setup lang="ts">
import { computed } from 'vue'
import { Button } from '@/components/shadcn/ui/button'
import { toTypedSchema } from '@vee-validate/zod'
import {
  ErrorMessage,
  Field,
  useForm
} from 'vee-validate'
import { z } from 'zod'

const validationSchema = toTypedSchema(
  z
    .object({
      nombre: z.string({
        message: 'El nombre es requerido',
      }).min(1,{
        message: 'El nombre debe tener al menos un caracter',
      }),
      correo: z.string({
        message: 'El correo es requerido',
      }).email({
        message: 'El correo debe ser válido',
      }),
      password: z.string({
        message: 'La contraseña es requerida',
      }).min(6,{
        message: 'La contraseña debe tener al menos 6 caracteres',
      }),
      confirm: z.string({
        message: 'La confirmación de la contraseña es requerida',
      }),
    })
    .refine((data) => data.password === data.confirm, {
      message: "Las contraseñas no coinciden",
      path: ['confirm'],
    }),
)
const form = useForm({
  validationSchema,
})

const onSubmit = form.handleSubmit((values) => {
  alert('El registro se ha realizado correctamente')
  form.resetForm()
})
</script>

<template>
  <div class="flex flex-col items-center justify-center w-full h-screen">
    <form @submit.prevent="onSubmit" class="flex flex-col gap-4 w-full max-w-xl">
      <Field v-slot="{ field }" name="nombre" class="grid w-full items-center gap-1.5">
        <Label for="nombre">Nombre</Label>
        <Input v-bind="field" id="nombre" type="text" />
        <ErrorMessage class="text-red-600" name="nombre" />
      </Field>
      <Field v-slot="{ field }" name="correo" class="grid w-full items-center gap-1.5">
        <Label for="email">Correo</Label>
        <Input v-bind="field" id="email" type="email" />
        <ErrorMessage class="text-red-600" name="correo" />
      </Field>
      <Field v-slot="{ field }" name="password" class="grid w-full items-center gap-1.5">
        <Label for="pw1">Contraseña</Label>
        <Input v-bind="field" id="pw1" type="password" />
        <ErrorMessage class="text-red-600" name="password" />
      </Field>
      <Field v-slot="{ field }" name="confirm" class="grid w-full items-center gap-1.5">
        <Label for="pw2">Repetir Contraseña</Label>
        <Input v-bind="field" id="pw2" type="password" />
        <ErrorMessage class="text-red-600" name="confirm" />
      </Field>
      <Button type="submit" variant="default" class="w-fit bg-green-600 text-white px-8">Enviar </Button>
    </form>
  </div>
</template>
