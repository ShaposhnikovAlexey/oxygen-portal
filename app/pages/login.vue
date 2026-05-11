<script setup lang="ts">
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

definePageMeta({
  layout: 'auth'
})

useSeoMeta({
  title: 'Login',
  description: 'Login to your account to continue'
})

const toast = useToast()

const fields = [{
  name: 'email',
  type: 'text' as const,
  label: 'Email',
  placeholder: 'Введите Email',
  required: true
}, {
  name: 'password',
  label: 'Пароль',
  type: 'password' as const,
  placeholder: 'Введите свой пароль'
}, {
  name: 'remember',
  label: 'Запомнить меня',
  type: 'checkbox' as const
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
    title="Добро пожаловать на портал"
    icon="i-lucide-lock"
    :submit="{ label: 'Продолжить' }"
    @submit="onSubmit"
  >
    <template #description>
      Еще нет аккаунта? <ULink
        to="/signup"
        class="text-primary font-medium"
      >Регистрация</ULink>.
    </template>

    <template #password-hint>
      <ULink
        to="/"
        class="text-primary font-medium"
        tabindex="-1"
      >Забыли пароль?</ULink>
    </template>

    <template #footer>
      Авторизуясь, вы соглашаетесь с <ULink
        to="/"
        class="text-primary font-medium"
      >Условиями использования</ULink>.
    </template>
  </UAuthForm>
</template>
