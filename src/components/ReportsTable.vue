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
        <v-col cols="12" sm="4">
          <v-select
            :items="['all', 'extended', 'intermediate', 'primary']"
            label="Report types"
            v-model="filters.type"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="4">
          <v-select
            :items="['all', 'published', 'unpublished']"
            label="Published"
            v-model="filters.published"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="4">
          <p>Score range</p>
          <v-range-slider
          v-model="filters.range"
          hide-details
          dense
          :min="50"
          :max="150">
          <template v-slot:prepend>
            <v-text-field
            :min="50"
            :max="150"
            hide-details
            single-line
            style="width: 50px"
            v-model="filters.range[0]"
            type="number">
            </v-text-field>
          </template>
          <template v-slot:append>
            <v-text-field
            :min="50"
            :max="150"
            hide-details
            single-line
            style="width: 50px"
            v-model="filters.range[1]"
            type="number">
            </v-text-field>
          </template>
          </v-range-slider>
        </v-col>
      </v-row>
    </v-container>
  </v-card-text>
  <v-data-table
    :headers="headers"
    :items="filtered"
    :loading="loading"
    :search="search"
  >
  <template v-slot:item.body.reportScore="{ item }">
    {{ item.body.reportScore.toFixed(2) }}
  </template>
  </v-data-table>
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
      range: [50, 150],
    },
  }),
  computed: {
    filtered() {
      return this.reports
        .filter((report) => {
          const filters = (this.filterType(report)
          && this.filterPublished(report))
          && this.filterScore(report);
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
    filterScore(report) {
      const [min, max] = this.filters.range;
      const score = report.body.reportScore;
      return (score >= min && score <= max);
    },
  },
};
</script>
