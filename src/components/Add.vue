<template>
  <div>
    <v-container fluid>
      <v-row align="center" justify="center" class="mx-auto">
        <v-col cols="9">
          <v-row>
            <v-col><p class="text-h5 text-center">Users</p></v-col>
            <v-col v-for="field in this.fields" :key="field.value"
              ><p class="text-h5 text-center">{{ field.text }}</p></v-col
            >
          </v-row>
          <v-row
            align="center"
            justify="center"
            v-for="user in this.users"
            :key="user"
          >
            <v-col
              ><p class="text-h6 text-center">{{ user }}</p></v-col
            >
            <v-col v-for="field in fields" :key="field.text"
              ><v-text-field
                :type="field.type"
                v-model="today_data[user][field.text]"
              ></v-text-field
            ></v-col>
          </v-row>
          <v-row>
            <v-col justify="center" align="center"
              ><v-btn @click="submitRecord">Submit</v-btn></v-col
            >
          </v-row>
        </v-col>

        <v-col offset="1" cols="2" class="">
          <v-date-picker
            v-model="date"
            :max="maxDate"
            class="mt-4"
          ></v-date-picker>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-row>
        <v-col><v-btn @click="retrieve">Retrieve records</v-btn></v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
// import { getDatabase } from "firebase/database";
// const database = getDatabase();
// console.log(database)
import axios from "axios";

const today = new Date();

const months = [
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
];
const todaysDate = `${today.getFullYear()}-${
  today.getMonth() + 1
}-${today.getDate()}`;

export default {
  data() {
    return {
      today_data: {},
      date: todaysDate,
      maxDate: todaysDate,
      records: {
        2021: {
          January: {
            1: {
              Ahmad: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Rony: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Sonu: {
                mcq: 36,
                files: 2,
                records: 10,
              },
            },
            2: {
              Ahmad: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Rony: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Sonu: {
                mcq: 36,
                files: 2,
                records: 10,
              },
            },
          },
          February: {
            1: {
              Ahmad: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Rony: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Sonu: {
                mcq: 36,
                files: 2,
                records: 10,
              },
            },
            2: {
              Ahmad: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Rony: {
                mcq: 36,
                files: 2,
                records: 10,
              },

              Sonu: {
                mcq: 36,
                files: 2,
                records: 10,
              },
            },
          },
        },
      },
    };
  },
  methods: {
    printData() {
      console.log(this.today_data);
      console.log(this.date);
    },
    retrieve() {
      axios
        .get("https://reports-4888c-default-rtdb.firebaseio.com/records/2021/January/1.json")
        .then((res) => {
          let userArray = []
           
          for(let key in res.data){
             let records = []
            let user = key
            const data = res.data[key]
            for(let i in data){
              

             records.push(data[i])
            }
            userArray.push([user, ...records])
          }
          console.log(userArray)
        })
        .catch((er) => console.log(er));
    },
    submitRecord() {
      const dateToAdd = new Date(this.date);
      const year = dateToAdd.getFullYear();
      const month = months[dateToAdd.getMonth()];
      const day = dateToAdd.getDate();

      if (year in this.records) {
        console.log("year found" + year);
        if (!(month in this.records[year])) {
          this.records[year][month] = {};
          console.log("month created");
        }
      } else {
        console.log("year not found");
        this.records[year] = {};
        this.records[year][month] = {};
        console.log("crweated" + this.records[year][month]);
      }

      this.records[year][month][day] = this.today_data;
     
      axios
        .put(
          "https://reports-4888c-default-rtdb.firebaseio.com/records.json",this.records
        )
        .then((res) => {
          console.log(res);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  computed: {
    fields() {
      console.log(this.$store.state.fields);
      return this.$store.state.fields;
    },
    users() {
      return this.$store.state.users;
    },
  },
  created() {
    const x = this;
    this.users.forEach((user) => {
      x.today_data[user] = {};
      x.fields.forEach((field) => {
        x.today_data[user][field.value] = undefined;
      });
    });
  },
};
</script>

<style lang="scss" scoped>
</style>