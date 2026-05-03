<script setup lang="ts">
import type { UsageWindow } from '~/types/user-service'

defineProps<{
  window: UsageWindow
}>()
</script>

<template>
  <DashboardBreakdownWidget
    :window="window"
    endpoint="/api/v1/me/dashboard/top-models"
    icon="mdi-cube-outline"
    title="Top models"
    subtitle="By token volume"
    empty-label="No model usage yet"
  />
</template>
