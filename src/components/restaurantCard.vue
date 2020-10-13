<template>
  <v-card :to="urlDetail">
      <v-card-title>{{ restaurant.name }}</v-card-title>
      <v-card-text>
          <v-row>
              <v-col cols="6" xs="12">
                  Cuisine {{ restaurant.cuisine }}
              </v-col>
              <v-col cols="6" xs="12">
                {{ (restaurant.borough) ? restaurant.borough : '???' }}
              </v-col>
          </v-row>
      </v-card-text>
  </v-card>
</template>

<script>
export default {
    props: {
        restaurant: {
            type: Object,
            required: true,
            default: ()=>{
                return {}
            }
        }
    },
    data: function(){
        return {
            urlDetail: `/RestaurantDetail?id=${this.restaurant.id}`
        }
    }
}
</script>

<style>

</style>