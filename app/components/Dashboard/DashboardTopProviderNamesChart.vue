<script setup lang="ts">
import type { UsageWindow } from '~/types/user-service'

defineProps<{
  window: UsageWindow
}>()
</script>

<template>
  <DashboardBreakdownWidget
    :window="window"
    endpoint="/api/v1/me/dashboard/top-provider-names"
    chart-type="pie"
    icon="mdi-cloud-outline"
    title="Top provider names"
    subtitle="By token volume"
    empty-label="No provider usage yet"
  />
</template>
