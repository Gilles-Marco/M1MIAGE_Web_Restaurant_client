<template>
  <view-with-drawer>
    <confirm ref="confirmDialog" :title="confirmVar.title" :message="confirmVar.message" 
      :confirmColor="confirmVar.confirmColor" :cancelButtonText="confirmVar.cancelButtonText" 
      :confirmButtonText="confirmVar.confirmButtonText" :confirmButtonAction="confirmVar.confirmButtonAction" 
      :cancelButtonAction="confirmVar.cancelButtonAction" :args="confirmVar.args"/>
    <restaurant-crud ref="restaurant_crud"/>
    <advanced-search ref="advanced_search" @advanced-search-restaurants="displayAdvancedSearch"/>
    <v-container class="ml-3 mt-3">
      <v-row>
        <v-col cols="9">
          <v-text-field
            v-model="search"
            label="Search"
            prepend-inner-icon="mdi-magnify"
            width="75%"
            v-show="(advancedRestaurants.length == 0)"
          ></v-text-field>
          <v-btn color="purple" class="white--text" v-show="(advancedRestaurants.length > 0)">
            1 Recherche avancée
            <v-btn icon @click="advancedRestaurants = []; pageControler.page = 1"><v-icon color="white">mdi-close-octagon</v-icon></v-btn>
          </v-btn>
        </v-col>
        <v-col cols="3">
          <v-btn color="primary" class="mt-5 ml-2" @click="openSearch()">Recherche avancée <v-icon></v-icon></v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col
          lg="6"
          md="12"
          v-for="restaurant in restaurantToDisplay"
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
import RestaurantCrud from '@/components/restaurantCrud.vue'
import AdvancedSearch from '@/components/advanced_search.vue'

export default {
  components: { ViewWithDrawer, RestaurantCard, PageControler, Confirm, RestaurantCrud, AdvancedSearch },
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
      },
      advancedRestaurants: [],
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
      this.confirmVar.title = "Vous êtes sur le point de supprimer une entrée"
      this.confirmVar.message = "Vous ne pourrez pas revenir en arrière, une fois celle-ci confirmée"
      this.confirmVar.confirmColor = "red"
      this.confirmVar.confirmButtonAction = this.deleteRestaurant
      this.confirmVar.confirmButtonText = "SUPPRIMER"
      this.confirmVar.cancelButtonText = "Annuler"
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
      this.$refs.restaurant_crud.openDialogEdit(restaurant)
    },
    closeDialog(){
      this.$refs.confirmDialog.showDialog = false
    },
    openSearch(){
      this.$refs.advanced_search.openDialog()
    },
    displayAdvancedSearch(restaurants){
      this.advancedRestaurants = restaurants
      this.pageControler.page = 1
    }
  },
  computed: {
    filteredRestaurants() {
      return this.restaurants.filter((r) => r.name.match(this.search) || r.cuisine.match(this.search) || r.borough.match(this.search));
    },
    restaurantToDisplay(){
      // Priority is accorded to advanced search then to filteredRestaurant then to normal display
      if (this.advancedRestaurants.length > 0) {
        // Paginate the advanced result if necessary
        if (this.advancedRestaurants.length > this.pageControler.pagesize){
          let start = (this.pageControler.page-1) * this.pageControler.pagesize
          let end = start + this.pageControler.pagesize
          let sub_selection = this.advancedRestaurants.slice(start, end)
          return sub_selection
        }
        // No pagination because it is not necessary
        else
          return this.advancedRestaurants
      }
      else if (this.filteredRestaurants.length > 0) return this.filteredRestaurants
      else return this.restaurants
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