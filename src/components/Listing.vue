<template>
  <div>
    <v-container v-if="!editComp" fluid>
      <v-data-table 
      :headers="headers" :items="getListingData"  :loading="loading" sort-by="id"
      :server-items-length="getCount"
        class="elevation-1 ma-3">
        <template v-slot:top>
          <!---------Tool Bar----------->
          <v-toolbar color="primary" flat>
            <v-toolbar-title class="white--text">Products Listing
            </v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ props }">
                <v-btn color="success" dark class="mb-2" v-bind="props" @click="createNew">
                  Create New
                </v-btn>
              </template>
            </v-dialog>
            <v-dialog v-model="dialogDelete" max-width="500px">
              <v-card>
                <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
                  <v-btn color="error" variant="text" @click="deleteItemConfirm">OK</v-btn>
                  <v-spacer></v-spacer>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon size="small" class="me-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon size="small" @click="deleteItem(item)">
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
      <!-- <div class="d-flex justify-center">
      <v-pagination
        v-model="currentPage"
        :length="Math.ceil(getCount / itemsPerPage)"
        :total-visible="5"
        prev-icon="mdi-chevron-left"
        next-icon="mdi-chevron-right"
        @input="paginate"
      ></v-pagination>
    </div> -->
  </v-container>
    <div v-else>
      <Edit :userObj="userObj" @goBack="handleEmit" />
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import { mapActions, mapGetters } from "vuex";
import Edit from '@/components/Edit.vue'

export default {
  name: "Listing",
  components: { Edit },
  data: () => ({
    loading: false,
    editComp: false,
    userObj: {
    },
    delObj: {
    },
    dialog: false,
    search: "",
    dialogDelete: false,
    dialoginfo: false,
    headers: [
      {
        text: "Id",
        sortable: false,
        value: "id",
      },
      { text: "Name", value: "name", sortable: false, },
      { text: "Price", value: "items", sortable: false, },
      { text: "Discount", value: "discount", sortable: false, },
      { text: "Description", value: "description", sortable: false,},
      { text: "Actions", value: "actions", sortable: false, },

    ],
    donorId: -1,
    editedIndex: -1,
    donorDetail: {},
    currentPage: 1,
    itemsPerPage: 5,
  }),

  computed: {
    ...mapGetters(["getListingData","getCount"]),
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
    dialoginfo(val) {
      val || this.close();
    },
  },
  created() {
   
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    ...mapActions(["fetchListingData"]),

    fetchData() {
      this.loading=true;
      let data={
        currentPage:this.currentPage,
        itemsPerPage:this.itemsPerPage,
      }
      this.fetchListingData(data).then(
        (response) => {
          if (response.status == 200) {
            this.loading=false;
          }
        },
        (error) => {
          this.loading=false;
          console.log("Error:", error);
        }
      );


    },
    editItem(item) {
      this.userObj = {};
      this.userObj = item;
      this.editComp = true;
    },

    deleteItem(item) {
      this.delObj = {};
      this.delObj = item;
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.dialogDelete = false;
      axios.delete(`http://127.0.0.1:8000/api/products/${this.delObj.id}/delete`).then(res => {
        this.fetchData();
        alert("User Deleted Successfully!");


      }).catch(function (error) {
        this.dialogDelete = false;
        if (error.response) {
          if (error.response.data.status == 404) {
            alert(error.response.data.message);
          }
        }
        else {
          console.log('Error', error.message)
        }
      });
    },

    close() {
      this.dialog = false;
    },

    closeDelete() {
      this.dialogDelete = false;
    },

    createNew() {
      this.$router.push("/create")
    },
    handleEmit() {
      this.editComp = false;
      this.fetchData();
    },
    paginate(page) {
      this.currentPage = page;
      this.fetchData();
    },

  },
};
</script>
