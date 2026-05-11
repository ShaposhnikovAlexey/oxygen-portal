<script setup lang="ts">
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

definePageMeta({
  layout: 'auth'
})

useSeoMeta({
  title: 'Sign up',
  description: 'Create an account to get started'
})

const toast = useToast()

const fields = [{
  name: 'name',
  type: 'text' as const,
  label: 'Имя',
  placeholder: 'Введите ваше имя'
}, {
  name: 'email',
  type: 'text' as const,
  label: 'Email',
  placeholder: 'Введите ваш email'
}, {
  name: 'password',
  label: 'Пароль',
  type: 'password' as const,
  placeholder: 'Введите пароль'
}]

const providers = [{
  label: 'Google',
  icon: 'i-simple-icons-google',
  onClick: () => {
    toast.add({ title: 'Google', description: 'Login with Google' })
  }
}, {
  label: 'GitHub',
  icon: 'i-simple-icons-github',
  onClick: () => {
    toast.add({ title: 'GitHub', description: 'Login with GitHub' })
  }
}]

const schema = z.object({
  name: z.string().min(1, 'Name is required'),
  email: z.email('Invalid email'),
  password: z.string().min(8, 'Must be at least 8 characters')
})

type Schema = z.output<typeof schema>

function onSubmit(payload: FormSubmitEvent<Schema>) {
  console.log('Submitted', payload)
}
</script>

<template>
  <UAuthForm
    :fields="fields"
    :schema="schema"
    :providers="providers"
    title="Регистрация"
    :submit="{ label: 'Создать аккаунт' }"
    @submit="onSubmit"
  >
    <template #description>
      Уже есть аккаунт? <ULink
        to="/login"
        class="text-primary font-medium"
      >Войти</ULink>.
    </template>

    <template #footer>
      Регистрируясь, вы соглашаетесь с <ULink
        to="/"
        class="text-primary font-medium"
      >Условиями использования</ULink>.
    </template>
  </UAuthForm>
</template>
