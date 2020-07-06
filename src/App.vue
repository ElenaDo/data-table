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
    fetch('./reports.json')
      .then((response) => response.json())
      .then((data) => console.log(data));
  },
  data: () => ({
    loading: false,
    reports: [],
    //
  }),
};
</script>
