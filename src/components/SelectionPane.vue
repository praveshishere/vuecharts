<template>
  <div>
    <v-row class="mt-10">
      <v-col cols="3" justify="center" align="center">
        <p class="text-h4 mt-6">Select a Timespan:</p>
      </v-col>
      <v-col cols="2">
        <v-select
          :items="Object.keys(records)"
          v-model="year"
          class="ma-4 mt-6"
          solo
        >
        </v-select>
      </v-col>
      <v-col cols="2">
        <v-select
          :items="Object.keys(records[year])"
          v-model="month"
          class="ma-4 mt-6"
          solo
        >
        </v-select>
      </v-col>
      <v-col cols="2">
        <v-select
          :items="Object.keys(records[year][month])"
          v-model="day"
          class="ma-4 mt-6"
          solo
        >
        </v-select>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      day: "2",
      month: "January",
      year: "2021",
      months: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
      fieldsToShow: [],
      days: [],
    };
  },
};
</script>

<style lang="scss" scoped>
</style>