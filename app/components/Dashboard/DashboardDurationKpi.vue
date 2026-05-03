<script setup lang="ts">
import type {
  DashboardDurationResponse,
  UsageWindow,
} from '~/types/user-service'
import { formatDurationMs } from '~/utils/formatters'

const props = defineProps<{
  window: UsageWindow
}>()

const widget = useDashboardWidget<DashboardDurationResponse>(
  '/api/v1/me/dashboard/duration',
  () => props.window,
)

const totalDurationMs = computed(
  () => widget.data.value?.totalDurationMs ?? null,
)
</script>

<template>
  <DashboardKpiCard
    icon="mdi-timer-outline"
    color="success"
    title="Total duration"
    :value="totalDurationMs"
    :formatter="formatDurationMs"
    caption="Completed requests"
    :loading="widget.loading.value"
    :error="widget.error.value"
    @retry="widget.reload"
  />
</template>
