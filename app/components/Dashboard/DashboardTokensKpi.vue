<script setup lang="ts">
import type { DashboardTokensResponse, UsageWindow } from '~/types/user-service'
import { formatNumber } from '~/utils/formatters'

const props = defineProps<{
  window: UsageWindow
}>()

const widget = useDashboardWidget<DashboardTokensResponse>(
  '/api/v1/me/dashboard/tokens',
  () => props.window,
)

const totalTokens = computed(() => widget.data.value?.totalTokens ?? null)
const completionTokens = computed(
  () => widget.data.value?.completionTokens ?? 0,
)
const embeddingTokens = computed(() => widget.data.value?.embeddingTokens ?? 0)
const caption = computed(
  () =>
    `${formatNumber(completionTokens.value)} completion / ${formatNumber(
      embeddingTokens.value,
    )} embedding`,
)
</script>

<template>
  <DashboardKpiCard
    icon="mdi-counter"
    color="warning"
    title="Total tokens"
    :value="totalTokens"
    :formatter="formatNumber"
    :caption="caption"
    :loading="widget.loading.value"
    :error="widget.error.value"
    @retry="widget.reload"
  />
</template>
