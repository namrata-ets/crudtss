<!-- eslint-disable prettier/prettier -->
<!-- eslint-disable prettier/prettier -->
<!-- eslint-disable prettier/prettier -->
<!-- eslint-disable prettier/prettier -->
<!-- eslint-disable vue/require-v-for-key -->
<!-- eslint-disable vue/no-unused-vars -->
<!-- eslint-disable vue/valid-v-slot -->

<template>
  <v-container>
    <v-card>
      <v-card-title>
      Airline Reservation
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    </v-card>
    <v-data-table
      :headers="headers"
      :items="passengers"
      sort-by="name"
      :server-items-length="totalPassengers"
      class="elevation-1"
      :options.sync="options"
      :items-per-page="10"
      :search="search"
    >
      <template v-slot:item.airline="{ item }">
        <span v-for="name in item.airline" v-bind="item.airline.id">
          {{ name.name }}
        </span>
      </template>

      <template v-slot:top>
        <v-toolbar flat>
       
      <v-spacer></v-spacer>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                New Passenger
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.airline"
                        label="Airline name"
                        value=this.editedItem.airline
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.trips"
                        label="No Of Trips"
                        value="editedItem.trips"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.name"
                        label="Passenger Name"
                        value="editedItem.name"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5"
                >Are you sure you want to delete this item?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"
                  >Cancel</v-btn
                >
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                  >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
  </v-container>
</template>
<script>
import axios from "axios";
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    search: " ",
    airline: " ",
    // eslint-disable-next-line prettier/prettier
    options: {},
    loading: true,
    passengers: [],
    totalPassengers: 0,
    page: 0,
    headers: [
      {
        text: "Airline Name",
        align: "start",
        value: "airline",
      },
      { text: "Number of Trips", value: "trips" },
      { text: "Passenger Name", value: "name" },

      { text: "Actions", value: "actions", sortable: false },
    ],

    editedIndex: -1,

    editedItem: {
      name: " ",
      trips: 0,
      airline: " ",
    },
    defaultItem: {
      name: " ",
      trips: 0,
      airline: " ",
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Passenger" : "Edit Passenger";
    },
  },

  watch: {
    dialog(val) {
      console.log(val);
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
    options: {
      handler() {
        this.page += 1;
        this.getDataFromApi();
      },
      deep: true,
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.passengers = [];
    },
    getDataFromApi() {
      this.loading = true;
      this.getPassengersData();
    },
    async getPassengersData() {
      await axios
        .get(
          `https://api.instantwebtools.net/v1/passenger?size=10&page=` +
            this.page
        )
        .then((response) => {
          this.passengers = response.data.data;
          this.totalPassengers = response.data.totalPassengers;
          this.totalPages = response.data.totalPages;
          console.log("Total number of passengers is: " + this.totalPassengers);
        })
        .catch((error) => console.log(error));
      console.log(
        "Passenger data fetched from API is: " + JSON.stringify(this.passengers)
      );

      for (var i = 0; i < this.passengers.length; i++) {
        this.airlineName = this.passengers[i].airline;
        //this.name = this.passengers[i].name;
        //console.log(this.name);
        console.log(this.airlineName);

        for (var x in this.airlineName) {
          this.airline = this.airlineName[x].name;
          console.log(this.airline);
        }
      }
    },

    fakeApiCall() {
      return new Promise((resolve) => {
        let { sortBy, sortDesc, page, itemsPerPage } = this.options;

        let items = this.getPassengersData();
        const total = items.length;

        if (sortBy.length === 1 && sortDesc.length === 1) {
          items = this.passengers.sort((a, b) => {
            const sortA = a[sortBy[0]];
            const sortB = b[sortBy[0]];

            if (sortDesc[0]) {
              if (sortA < sortB) return 1;
              if (sortA > sortB) return -1;
              return 0;
            } else {
              if (sortA < sortB) return -1;
              if (sortA > sortB) return 1;
              return 0;
            }
          });
        }

        if (itemsPerPage > 0) {
          items = items.slice((page - 1) * itemsPerPage, page * itemsPerPage);
        }

        setTimeout(() => {
          resolve({
            items,
            total,
          });
        }, 1000);
      });
    },

    editItem(item) {
      this.editedIndex = this.passengers.indexOf(item);

      for (var i = 0; i < item.airline.length; i++) {
        console.log(item.airline[i].name);
        this.editedItem.airline = item.airline[i].name;
        this.editedItem.name = item.name;
        this.editedItem.trips = item.trips;
      }
      console.log(this.editedItem);
      this.editedItem = Object.assign({}, { item });

      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.passengers.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.passengers.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.passengers[this.editedIndex], this.editedItem);
      } else {
        this.passengers.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>
