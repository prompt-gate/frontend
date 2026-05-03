<script setup lang="ts">
import type { UserTokenPayload } from '~/types/user-service'

const props = defineProps<{
  loading: boolean
}>()

const emit = defineEmits<{
  create: [payload: UserTokenPayload]
}>()

const isOpen = defineModel<boolean>({ default: false })
const createFormKey = shallowRef(0)

watch(isOpen, (open) => {
  if (open) {
    createFormKey.value += 1
  }
})
</script>

<template>
  <AppDialogCard
    v-model="isOpen"
    icon="mdi-key-plus"
    max-width="760"
    title="Create virtual key"
    subtitle="Generate a virtual key for CLI, SDK, or proxy access."
    :loading="props.loading"
  >
    <AppTokenCreateForm
      :key="createFormKey"
      :autofocus="true"
      :inline="false"
      :loading="props.loading"
      @create="emit('create', $event)"
    />

    <template #actions>
      <v-spacer />
      <AppDialogCloseButton :disabled="props.loading" @click="isOpen = false" />
    </template>
  </AppDialogCard>
</template>
