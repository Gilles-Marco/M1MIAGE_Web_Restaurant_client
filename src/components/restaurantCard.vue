<template>
  <v-card :to="urlDetail">
      <v-card-title>{{ restaurant.name }}</v-card-title>
      <v-card-text>
          <v-row>
              <v-col cols="6" xs="12">
                  <restaurant-field title="Cuisine" :content="restaurant.cuisine"/>
              </v-col>
              <v-col cols="6" xs="12">
                <restaurant-field title="City" :content="(restaurant.borough) ? restaurant.borough : '???'"/>
              </v-col>
          </v-row>
      </v-card-text>
  </v-card>
</template>

<script>
import RestaurantField from '@/components/restaurantField.vue'

export default {
    components:{
        RestaurantField,
    },
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