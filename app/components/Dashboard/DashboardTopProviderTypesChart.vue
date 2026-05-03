<script setup lang="ts">
import type { UsageWindow } from '~/types/user-service'

defineProps<{
  window: UsageWindow
}>()
</script>

<template>
  <DashboardBreakdownWidget
    :window="window"
    endpoint="/api/v1/me/dashboard/top-provider-types"
    chart-type="pie"
    icon="mdi-shape-outline"
    title="Top provider types"
    subtitle="By token volume"
    empty-label="No provider type usage yet"
  />
</template>
