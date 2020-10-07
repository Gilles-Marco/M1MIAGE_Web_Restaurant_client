<template>
  <v-app>
      <v-main>
          <drawer @displayPopupCreateRestaurant="openRestaurantCrudDialog"/>
          <slot></slot>
          <restaurant-crud ref="restaurant_crud"/>
      </v-main>
  </v-app>
</template>

<script>
import Drawer from '@/components/drawer.vue'
import restaurantCrud from '@/components/restaurantCrud.vue'

export default {
    components: {Drawer, restaurantCrud},
    methods: {
        openRestaurantCrudDialog(){
            this.$refs.restaurant_crud.openDialog()
        }
    }
}
</script>

<style>

</style>