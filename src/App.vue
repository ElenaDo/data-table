<template>
  <v-app>
    <v-content>
      <ReportsTable></ReportsTable>
    </v-content>
  </v-app>
</template>

<script>
import ReportsTable from '@/components/ReportsTable.vue';

export default {
  name: 'App',
  components: {
    ReportsTable,
  },
  data: () => ({
    //
  }),
};
</script>
