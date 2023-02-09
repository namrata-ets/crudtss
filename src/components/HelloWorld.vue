<!-- eslint-disable vue/require-v-for-key -->
<!-- eslint-disable vue/no-unused-vars -->
<!-- eslint-disable vue/valid-v-slot -->

<template>
  <v-container>
    <v-data-table
      :headers="headers"
      :items="passengers"
      sort-by="trips"
      server-items-length="66586"
      class="elevation-1"
    >
      <template v-slot:item.airline="{ item }">
        <span v-for="name in item.airline" v-bind="item.airline.id">
          {{ name.name }}
        </span>
      </template>

      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Airline Reservation</v-toolbar-title>
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
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.trips"
                        label="No Of Trips"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.name"
                        label="Passenger Name"
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
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    name: "",
    airline: " ",
    headers: [
      {
        text: "Airline Name",
        align: "start",
        sortable: false,
        value: "airline",
      },
      { text: "Number of Trips", value: "trips" },
      { text: "Passenger Name", value: "name" },

      { text: "Actions", value: "actions", sortable: false },
    ],
    passengers: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      trips: 0,
      airline: 0,
    },
    defaultItem: {
      name: "",
      trips: 0,
      airline: 0,
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
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.passengers = [
        {
          _id: "63e34c2d87ade9831e0cbd69",
          name: "Harish",
          trips: 230,
          airline: [
            {
              id: 4,
              name: "Emirates",
              country: "Dubai",
              logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Emirates_logo.svg/150px-Emirates_logo.svg.png",
              slogan: "From Dubai to destinations around the world.",
              head_quaters: "Garhoud, Dubai, United Arab Emirates",
              website: "www.emirates.com/",
              established: "1985",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade91cd70cbd6d",
          name: "RAJESH",
          trips: 234,
          airline: [
            {
              id: 5,
              name: "Eva Air",
              country: "Taiwan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/ed/EVA_Air_logo.svg/250px-EVA_Air_logo.svg.png",
              slogan: "Sharing the World, Flying Together",
              head_quaters:
                "376, Hsin-Nan Rd., Sec. 1, Luzhu, Taoyuan City, Taiwan",
              website: "www.evaair.com",
              established: "1989",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade912620cbd6f",
          name: "AJITHA",
          trips: 345,
          airline: [
            {
              id: 9,
              name: "Japan Airlines",
              country: "Japan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/d/dd/Japan_Airlines_Logo_%282011%29.svg/300px-Japan_Airlines_Logo_%282011%29.svg.png",
              slogan: "Fly into tomorrow",
              head_quaters: "Shinagawa, Tokyo, Japan",
              website: "www.jal.com",
              established: "1951",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade9b1030cbd72",
          name: "Harish",
          trips: 230,
          airline: [
            {
              id: 4,
              name: "Emirates",
              country: "Dubai",
              logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Emirates_logo.svg/150px-Emirates_logo.svg.png",
              slogan: "From Dubai to destinations around the world.",
              head_quaters: "Garhoud, Dubai, United Arab Emirates",
              website: "www.emirates.com/",
              established: "1985",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade911bd0cbd76",
          name: "AJIT",
          trips: 345,
          airline: [
            {
              id: 9,
              name: "Japan Airlines",
              country: "Japan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/d/dd/Japan_Airlines_Logo_%282011%29.svg/300px-Japan_Airlines_Logo_%282011%29.svg.png",
              slogan: "Fly into tomorrow",
              head_quaters: "Shinagawa, Tokyo, Japan",
              website: "www.jal.com",
              established: "1951",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade932fd0cbd78",
          name: "RAJESH",
          trips: 234,
          airline: [
            {
              id: 5,
              name: "Eva Air",
              country: "Taiwan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/ed/EVA_Air_logo.svg/250px-EVA_Air_logo.svg.png",
              slogan: "Sharing the World, Flying Together",
              head_quaters:
                "376, Hsin-Nan Rd., Sec. 1, Luzhu, Taoyuan City, Taiwan",
              website: "www.evaair.com",
              established: "1989",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade943f10cbd7b",
          name: "Harish",
          trips: 230,
          airline: [
            {
              id: 4,
              name: "Emirates",
              country: "Dubai",
              logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Emirates_logo.svg/150px-Emirates_logo.svg.png",
              slogan: "From Dubai to destinations around the world.",
              head_quaters: "Garhoud, Dubai, United Arab Emirates",
              website: "www.emirates.com/",
              established: "1985",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade90baf0cbd7f",
          name: "RAJESH",
          trips: 234,
          airline: [
            {
              id: 5,
              name: "Eva Air",
              country: "Taiwan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/ed/EVA_Air_logo.svg/250px-EVA_Air_logo.svg.png",
              slogan: "Sharing the World, Flying Together",
              head_quaters:
                "376, Hsin-Nan Rd., Sec. 1, Luzhu, Taoyuan City, Taiwan",
              website: "www.evaair.com",
              established: "1989",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34c2e87ade969e30cbd81",
          name: "AJITHA",
          trips: 345,
          airline: [
            {
              id: 9,
              name: "Japan Airlines",
              country: "Japan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/d/dd/Japan_Airlines_Logo_%282011%29.svg/300px-Japan_Airlines_Logo_%282011%29.svg.png",
              slogan: "Fly into tomorrow",
              head_quaters: "Shinagawa, Tokyo, Japan",
              website: "www.jal.com",
              established: "1951",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e34e0187ade954e10cbe18",
          name: "Prashant",
          trips: 25,
          airline: [
            {
              id: 5,
              name: "Eva Air",
              country: "Taiwan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/ed/EVA_Air_logo.svg/250px-EVA_Air_logo.svg.png",
              slogan: "Sharing the World, Flying Together",
              head_quaters:
                "376, Hsin-Nan Rd., Sec. 1, Luzhu, Taoyuan City, Taiwan",
              website: "www.evaair.com",
              established: "1989",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e4d663787533f77acb12c8",
          name: "testuser",
          trips: 15,
          airline: [
            {
              id: 10,
              name: "Swiss International Air Lines",
              country: "Switzerland",
              logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Swiss_International_Air_Lines_Logo_2011.svg/200px-Swiss_International_Air_Lines_Logo_2011.svg.png",
              slogan: "Made of Switzerland. The Airline of Switzerland",
              head_quaters:
                "EuroAirport Basel Mulhouse Freiburg near Basel, Switzerland",
              website: "www.swiss.com",
              established: "2002",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e4d6657875338ccccb12cd",
          name: "testupName",
          trips: 10,
          airline: [
            {
              id: 7,
              name: "Deutsche Lufthansa AG",
              country: "Germany",
              logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/Lufthansa_Logo_2018.svg/300px-Lufthansa_Logo_2018.svg.png",
              slogan: "Say yes to the world",
              head_quaters: "Cologne, Germany",
              website: "lufthansa.com",
              established: "1953",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e4d6687875335e69cb12d4",
          name: "testupdatedName",
          trips: 3,
          airline: [
            {
              id: 1,
              name: "Quatar Airways",
              country: "Quatar",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/9/9b/Qatar_Airways_Logo.svg/300px-Qatar_Airways_Logo.svg.png",
              slogan: "Going Places Together",
              head_quaters: "Qatar Airways Towers, Doha, Qatar",
              website: "www.qatarairways.com",
              established: "1994",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e4d66a78753322aecb12db",
          name: "",
          trips: 10,
          airline: [
            {
              id: 8,
              name: "Thai Airways",
              country: "Thailand",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/5/58/Thai_Airways_Logo.svg/200px-Thai_Airways_Logo.svg.png",
              slogan: "Smooth as Silk / I Fly THAI",
              head_quaters:
                "Jom Phol Subdistrict, Chatuchak, Bangkok, Thailand",
              website: "www.thaiairways.com",
              established: "1960",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e4d6707875334a98cb12e5",
          name: "TestUserName",
          trips: 3,
          airline: [
            {
              id: 5,
              name: "Eva Air",
              country: "Taiwan",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/ed/EVA_Air_logo.svg/250px-EVA_Air_logo.svg.png",
              slogan: "Sharing the World, Flying Together",
              head_quaters:
                "376, Hsin-Nan Rd., Sec. 1, Luzhu, Taoyuan City, Taiwan",
              website: "www.evaair.com",
              established: "1989",
            },
          ],
          __v: 0,
        },
        {
          _id: "63e4d67378753347efcb12ea",
          name: "TestUserName12",
          trips: 1,
          airline: [
            {
              id: 3,
              name: "Cathay Pacific",
              country: "Hong Kong",
              logo: "https://upload.wikimedia.org/wikipedia/en/thumb/1/17/Cathay_Pacific_logo.svg/300px-Cathay_Pacific_logo.svg.png",
              slogan: "Move Beyond",
              head_quaters:
                "Cathay City, Hong Kong International Airport, Chek Lap Kok, Hong Kong",
              website: "www.cathaypacific.com",
              established: "1946",
            },
          ],
          __v: 0,
        },
      ];
    },

    editItem(item) {
      this.editedIndex = this.passengers.indexOf(item);
      this.editedItem = Object.assign({}, item);
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
