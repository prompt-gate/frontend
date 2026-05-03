<script setup lang="ts">
import type { UsageWindow } from '~/types/user-service'

definePageMeta({
  icon: 'mdi-monitor-dashboard',
  title: 'Dashboard',
  drawerIndex: 0,
  requiredRoles: ['user', 'manager', 'admin'],
})

const selectedWindow = shallowRef<UsageWindow>('7d')
</script>

<template>
  <v-container fluid class="app-page user-dashboard-page">
    <div class="user-dashboard-page__header">
      <div>
        <h1 class="user-dashboard-page__title">My Dashboard</h1>
        <p class="user-dashboard-page__subtitle">
          Your service usage across the selected period.
        </p>
      </div>

      <DashboardTimeRangeSelect v-model="selectedWindow" />
    </div>

    <v-row>
      <v-col cols="12" md="4">
        <DashboardTokensKpi :window="selectedWindow" />
      </v-col>
      <v-col cols="12" md="4">
        <DashboardMessagesKpi :window="selectedWindow" />
      </v-col>
      <v-col cols="12" md="4">
        <DashboardDurationKpi :window="selectedWindow" />
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <DashboardActivityChart :window="selectedWindow" />
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12" lg="4">
        <DashboardTopModelsChart :window="selectedWindow" />
      </v-col>
      <v-col cols="12" lg="4">
        <DashboardTopProviderNamesChart :window="selectedWindow" />
      </v-col>
      <v-col cols="12" lg="4">
        <DashboardTopProviderTypesChart :window="selectedWindow" />
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped>
.user-dashboard-page__header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 16px;
  margin-bottom: 24px;
}

.user-dashboard-page__title {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 700;
}

.user-dashboard-page__subtitle {
  margin: 4px 0 0;
  color: rgb(var(--app-shell-text-secondary));
}

@media (max-width: 720px) {
  .user-dashboard-page__header {
    align-items: stretch;
    flex-direction: column;
  }
}
</style>
