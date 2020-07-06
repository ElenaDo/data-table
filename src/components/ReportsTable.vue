<template>
  <v-data-table
    :headers="headers"
    :items="reports"
    :loading="loading"
  ></v-data-table>
</template>

<script>
export default {
  name: 'ReportsTable',
  props: {
    loading: { type: Boolean, required: true },
    reports: { type: Array, required: true },
  },
  data: () => ({
    headers: [
      // { text: 'id', value: 'uuid' },
      { text: 'Bank Name', value: 'body.bankName' },
      { text: 'BIC', value: 'body.bankBIC' },
      { text: 'Score', value: 'body.reportScore' },
      { text: 'Type', value: 'body.type' },
      { text: 'Created', value: 'createdAt' },
      { text: 'Published', value: 'publishedAt' },
    ],
  }),
};
</script>
