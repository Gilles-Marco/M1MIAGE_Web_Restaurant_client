<template>
  <view-with-drawer>
    <confirm ref="confirmDialog" :title="confirmVar.title" :message="confirmVar.message" 
      :confirmColor="confirmVar.confirmColor" :cancelButtonText="confirmVar.cancelButtonText" 
      :confirmButtonText="confirmVar.confirmButtonText" :confirmButtonAction="confirmVar.confirmButtonAction" 
      :cancelButtonAction="confirmVar.cancelButtonAction" :args="confirmVar.args"/>
    <v-container class="ml-3 mt-3">
      <v-row
        ><v-text-field
          v-model="search"
          label="Search"
          prepend-inner-icon="mdi-magnify"
          width="75%"
        ></v-text-field>
        <v-btn color="primary" class="mt-5 ml-2">Advanced Search</v-btn>
      </v-row>
      <v-row>
        <v-col
          lg="6"
          md="12"
          v-for="restaurant in filteredRestaurants"
          :key="restaurant.id"
          class="my-3"
        >
          <restaurant-card :showMenu="true" :restaurant="restaurant" @delete-restaurant="confirmRestaurantDeletion" @edit-restaurant="editRestaurant"/>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <page-controler v-model="pageControler" />
        </v-col>
      </v-row>
    </v-container>
  </view-with-drawer>
</template>

<script>
import ViewWithDrawer from "@/layout/view_with_drawer.vue";
import RestaurantCard from "@/components/restaurantCard.vue";
import PageControler from "@/components/pagecontroler.vue";
import Confirm from '@/components/confirm.vue';

export default {
  components: { ViewWithDrawer, RestaurantCard, PageControler, Confirm },
  data: function () {
    return {
      restaurants: [],
      pageControler: {
        page: 1,
        pagesize: 10,
      },
      search: "",
      voidFunction: function(){},
      confirmVar: {
        title: 'Confirm popup',
        message: 'Confirm your action',
        confirmColor: 'red',
        cancelButtonAction: this.voidFunction,
        confirmButtonAction: this.voidFunction,
        cancelButtonText: 'Cancel',
        confirmButtonText: 'Confirm',
        args: []
      }
    };
  },
  methods: {
    getRestaurant() {
      fetch(
        `http://localhost:8080/api/restaurants/?page=${this.pageControler.page}&pagesize=${this.pageControler.pagesize}`
      )
        .then((response) => {
          response.json().then((value) => {
            this.restaurants = value.data;
          });
        })
        .catch((error) => {
          console.error(error);
        });
    },
    confirmRestaurantDeletion(restaurant){
      this.confirmVar.title = "You are about to delete an entry"
      this.confirmVar.message = "You cannot undone this action when it's done"
      this.confirmVar.confirmColor = "red"
      this.confirmVar.confirmButtonAction = this.deleteRestaurant
      this.confirmVar.confirmButtonText = "DELETE"
      this.confirmVar.cancelButtonText = "Cancel"
      this.confirmVar.cancelButtonAction = this.closeDialog
      this.confirmVar.args = [restaurant]
      this.$refs.confirmDialog.openDialog()
    },
    deleteRestaurant(restaurants){
      return new Promise((resolve, reject)=>{
        fetch(`http://localhost:8080/api/restaurants/${restaurants[0]._id}`,  {
          method: 'DELETE'
        }).then((response)=>{
          this.getRestaurant()
          resolve(response)
        }).catch((error)=>{
          reject(error)
        })
      })
    },
    editRestaurant(restaurant){
      console.log('edit restaurant '+restaurant)
    },
    closeDialog(){
      this.$refs.confirmDialog.showDialog = false
    }
  },
  computed: {
    filteredRestaurants() {
      return this.restaurants.filter((r) => r.name.match(this.search));
    },
  },
  mounted() {
    this.getRestaurant();
    this.$watch("pageControler.page", () => {
      this.getRestaurant();
    });
    this.$watch("pageControler.pagesize", () => {
      this.getRestaurant();
    });
  },
};
</script>

<style>
</style>