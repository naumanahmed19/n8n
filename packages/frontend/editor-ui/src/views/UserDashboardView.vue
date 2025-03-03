<script setup lang="ts">
import { computed, onMounted, ref } from 'vue';
import { useI18n } from '@/composables/useI18n';
import { useUsersStore } from '@/stores/users.store';
import { useToast } from '@/composables/useToast';
import { useDocumentTitle } from '@/composables/useDocumentTitle';

const usersStore = useUsersStore();
const toast = useToast();
const i18n = useI18n();
const documentTitle = useDocumentTitle();

// User data
const userData = computed(() => usersStore.currentUser);
const isLoading = ref(false);

// Analytics data
const statsData = ref({
  workflows: 0,
  executions: 0,
  successfulExecutions: 0,
  failedExecutions: 0
});

onMounted(() => {
  documentTitle.set(i18n.baseText('userDashboard.title'));
  fetchDashboardData();
});

async function fetchDashboardData() {
  isLoading.value = true;
  try {
    // In a real implementation, you would fetch data from a backend API
    // This is just a placeholder
    setTimeout(() => {
      statsData.value = {
        workflows: 12,
        executions: 156,
        successfulExecutions: 142,
        failedExecutions: 14
      };
      isLoading.value = false;
    }, 500);
  } catch (error) {
    toast.showError(error, i18n.baseText('userDashboard.error.fetchFailed'));
    isLoading.value = false;
  }
}
</script>

<template>
  <div :class="$style.container">
    <div :class="$style.header">
      <h1>{{ i18n.baseText('userDashboard.title') }}</h1>
      <p v-if="userData">{{ i18n.baseText('userDashboard.welcome', { name: userData.firstName }) }}</p>
    </div>

    <n8n-loading :loading="isLoading" :rows="4" variant="p">
      <div :class="$style.statsGrid">
        <div :class="$style.statsCard">
          <span :class="$style.statsNumber">{{ statsData.workflows }}</span>
          <span :class="$style.statsLabel">{{ i18n.baseText('userDashboard.stats.workflows') }}</span>
        </div>
        <div :class="$style.statsCard">
          <span :class="$style.statsNumber">{{ statsData.executions }}</span>
          <span :class="$style.statsLabel">{{ i18n.baseText('userDashboard.stats.executions') }}</span>
        </div>
        <div :class="$style.statsCard">
          <span :class="$style.statsNumber">{{ statsData.successfulExecutions }}</span>
          <span :class="$style.statsLabel">{{ i18n.baseText('userDashboard.stats.successfulExecutions') }}</span>
        </div>
        <div :class="$style.statsCard">
          <span :class="$style.statsNumber">{{ statsData.failedExecutions }}</span>
          <span :class="$style.statsLabel">{{ i18n.baseText('userDashboard.stats.failedExecutions') }}</span>
        </div>
      </div>

      <div :class="$style.recentActivity">
        <h2>{{ i18n.baseText('userDashboard.recentActivity') }}</h2>
        <p>{{ i18n.baseText('userDashboard.comingSoon') }}</p>
      </div>
    </n8n-loading>
  </div>
</template>

<style lang="scss" module>
.container {
  height: 100%;
  width: 100%;
  padding: var(--spacing-2xl);
  overflow: auto;
}

.header {
  margin-bottom: var(--spacing-2xl);

  h1 {
    margin-bottom: var(--spacing-xs);
    font-size: var(--font-size-xl);
    font-weight: var(--font-weight-bold);
  }

  p {
    color: var(--color-text-base);
  }
}

.statsGrid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: var(--spacing-l);
  margin-bottom: var(--spacing-2xl);
}

.statsCard {
  background-color: var(--color-background-light);
  border-radius: var(--border-radius-base);
  padding: var(--spacing-l);
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.statsNumber {
  font-size: var(--font-size-2xl);
  font-weight: var(--font-weight-bold);
  color: var(--color-primary);
}

.statsLabel {
  font-size: var(--font-size-s);
  color: var(--color-text-base);
  margin-top: var(--spacing-xs);
}

.recentActivity {
  background-color: var(--color-background-light);
  border-radius: var(--border-radius-base);
  padding: var(--spacing-l);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);

  h2 {
    margin-bottom: var(--spacing-m);
    font-size: var(--font-size-l);
    font-weight: var(--font-weight-bold);
  }
}
</style>