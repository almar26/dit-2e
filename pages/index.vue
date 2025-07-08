<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-data-table
        :headers="headers"
        :items="customers"
        :items-per-page="10"
        class="elevation-1"
        :loading="loading"
        loading-text="Loading... Please wait"
      >
        <template v-slot:item.profilePicture="{ item }">
          <v-badge
            color="purple"
            overlap
            content="VIP"
            offset-x="10"
            offset-y="40"
            bordered
            v-if="item.isVip == true"
          >
            <v-avatar size="60">
              <v-img
                lazy-src="https://picsum.photos/id/11/10/6"
                :src="item.profilePicture"
              ></v-img>
            </v-avatar>
          </v-badge>
          <div v-else>
            <v-avatar size="60">
              <v-img
                lazy-src="https://picsum.photos/id/11/10/6"
                :src="item.profilePicture"
              ></v-img>
            </v-avatar>
          </div>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-btn color="primary" @click="getCustomerDetails(item)">View</v-btn>
        </template>
      </v-data-table>

      <v-dialog v-model="customerDialog" width="500">
        <v-card>
          <v-toolbar color="primary"
            ><v-toolbar-title
              ><v-icon left>mdi-information</v-icon> Customer
              Information</v-toolbar-title
            >
            <v-spacer></v-spacer>
            <v-btn @click="customerDialog = false" icon><v-icon>mdi-close</v-icon></v-btn>
            </v-toolbar
          >
          <v-card-text class="mt-5">
            <v-row>
              <v-col cols="12" md="4">
                <!-- Picture -->
                <v-img
                  max-height="250"
                  max-width="250"
                  :src="customerPhoto"
                ></v-img>
              </v-col>

              <v-col cols="12" md="8">
                <!-- Customer Information -->
                <v-list dense>
                  <v-list-item two-line>
                    <v-list-item-content>
                      <v-list-item-subtitle
                        >Name</v-list-item-subtitle
                      >
                      <v-list-item-title> {{ customerName }} </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>

                  <v-list-item two-line>
                    <v-list-item-content>
                      <v-list-item-subtitle
                        >Email</v-list-item-subtitle
                      >
                      <v-list-item-title>{{ customerEmail }}</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>

                  <v-list-item two-line>
                    <v-list-item-content>
                      <v-list-item-subtitle
                        >Phone #</v-list-item-subtitle
                      >
                      <v-list-item-title> {{ customerPhone }}</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>

                  <v-list-item two-line>
                    <v-list-item-content>
                      <v-list-item-subtitle
                        >Age</v-list-item-subtitle
                      >
                      <v-list-item-title>{{ customerAge }}</v-list-item-title>
                    
                    </v-list-item-content>
                    
                  </v-list-item>
                </v-list>
              </v-col>
            </v-row>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions class="py-5">
            <v-spacer></v-spacer>
            <v-btn color="primary"  @click="dialog = false">
              Update
            </v-btn>
            <v-btn color="error"  @click="dialog = false">
              Delete
            </v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-dialog>

    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      headers: [
        { text: "Photo", value: "profilePicture" },
        { text: "Name", value: "name" },
        { text: "Email", value: "email" },
        { text: "Phone", value: "phone" },
        { text: "Address", value: "address" },
        { text: "Age", value: "age" },
        { text: "Status", value: "isVip" },
        { text: "", value: "actions" },
      ],
      customers: [],
      loading: true,
      customerDialog: false,
      customerName: "",
      customerEmail: "",
      customerPhone: "",
      customerAge: 0,
      customerPhoto: ""

    };
  },
  methods: {
    getCustomers() {
      this.$axios
        .get("/customers")
        .then((response) => {
          console.log(response.data);
          this.customers = response.data;
          this.loading = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },

    getCustomerDetails(item) {
      console.log(item);
      
      this.customerName = item.name
      this.customerEmail = item.email
      this.customerPhone = item.phone
      this.customerAge = item.age
      this.customerPhoto = item.profilePicture
      this.customerDialog = true;
    },
  },

  mounted() {
    this.getCustomers();
  },
};
</script>
