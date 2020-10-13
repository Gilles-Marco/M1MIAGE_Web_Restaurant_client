<template>
  <view-with-drawer>
      <v-container class="ml-3 mt-3">
        <v-row>
          <v-col lg="6" md="12" v-for="restaurant in restaurants" :key="restaurant.id" class="my-3">
            <restaurant-card :restaurant="restaurant"/>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <page-controler v-model="pageControler"/>
          </v-col>
        </v-row>
      </v-container>
  </view-with-drawer>
</template>

<script>
import ViewWithDrawer from '@/layout/view_with_drawer.vue'
import RestaurantCard from '@/components/restaurantCard.vue'
import PageControler from '@/components/pagecontroler.vue'

export default {
    components: {ViewWithDrawer, RestaurantCard, PageControler},
    data: function(){
      return {
        restaurants: [],
        pageControler: {
          page: 1,
          pagesize: 10
        }
      }
    },
    methods: {
      getRestaurant(){
        fetch(`http://localhost:8080/api/restaurants/?page=${this.pageControler.page}&pagesize=${this.pageControler.pagesize}`).then((response)=>{
          response.json().then((value)=>{
            this.restaurants = value.data
            console.log(this.restaurants)
          })
        }).catch((error)=>{
          console.error(error)
        })
      }
    },
    mounted(){
      this.getRestaurant()
      this.$watch('pageControler.page', ()=>{
        this.getRestaurant()
      })
      this.$watch('pageControler.pagesize', ()=>{
        this.getRestaurant()
      })
    },
}
</script>

<style>

</style>