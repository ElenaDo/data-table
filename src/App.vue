<template>
  <v-app>
    <v-main>
      <ReportsTable :loading="loading" :reports="reports"></ReportsTable>
    </v-main>
  </v-app>
</template>

<script>
import ReportsTable from '@/components/ReportsTable.vue';

export default {
  name: 'App',
  components: {
    ReportsTable,
  },
  mounted() {
    const url = './reports.json';
    this.loading = true;
    fetch(url)
      .catch((err) => console.error('network error', err))
      .then((response) => response.json())
      .catch((err) => console.error('invalid data', err))
      .then((data) => {
        if (data) {
          this.reports = data;
        }
      })
      .finally(() => { this.loading = false; });
  },
  data: () => ({
    loading: false,
    reports: [],
    //
  }),
};
</script>
