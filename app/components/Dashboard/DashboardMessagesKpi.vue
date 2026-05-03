<script setup lang="ts">
import type {
  DashboardMessagesResponse,
  UsageWindow,
} from '~/types/user-service'
import { formatNumber } from '~/utils/formatters'

const props = defineProps<{
  window: UsageWindow
}>()

const widget = useDashboardWidget<DashboardMessagesResponse>(
  '/api/v1/me/dashboard/messages',
  () => props.window,
)

const messages = computed(() => widget.data.value?.messages ?? null)
</script>

<template>
  <DashboardKpiCard
    icon="mdi-message-text-outline"
    color="primary"
    title="Messages"
    :value="messages"
    :formatter="formatNumber"
    caption="Requests handled"
    :loading="widget.loading.value"
    :error="widget.error.value"
    @retry="widget.reload"
  />
</template>
