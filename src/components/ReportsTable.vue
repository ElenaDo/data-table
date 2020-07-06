<template>
<v-card>
  <v-card-title>
    Reports
    <v-spacer></v-spacer>
    <v-text-field
      v-model="search"
      append-icon="mdi-magnify"
      label="Search"
      single-line
      hide-details
    >
    </v-text-field>
  </v-card-title>
  <v-data-table
    :headers="headers"
    :items="reports"
    :loading="loading"
    :search="search"
  ></v-data-table>
</v-card>
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
    search: '',
  }),
};
</script>
