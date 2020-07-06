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
  <v-card-text>
    <v-container fluid>
      <v-row>
        <v-col cols="12" sm="3">
          <v-select
            :items="['all', 'extended', 'intermediate', 'primary']"
            label="Report types"
            v-model="filters.type"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="3">
          <v-select
            :items="['all', 'published', 'unpublished']"
            label="Published"
            v-model="filters.published"
          ></v-select>
        </v-col>
      </v-row>
    </v-container>
  </v-card-text>
  <v-data-table
    :headers="headers"
    :items="filtered"
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
    filters: {
      type: 'all',
      published: 'all',
    },
  }),
  computed: {
    filtered() {
      return this.reports
        .filter((report) => {
          const filters = (this.filterType(report) && this.filterPublished(report));
          return filters;
        });
    },
  },
  methods: {
    filterType(report) {
      if (this.filters.type === 'all') return true;
      return report.body.type === this.filters.type;
    },
    isPublished(date) {
      const publishDate = new Date(date);
      const today = new Date();
      return (publishDate <= today);
    },
    filterPublished(report) {
      const filterVal = this.filters.published;
      const { publishedAt } = report;
      if (filterVal === 'all') return true;
      if (filterVal === 'published') {
        return this.isPublished(publishedAt);
      }
      return !this.isPublished(publishedAt);
    },
  },
};
</script>
