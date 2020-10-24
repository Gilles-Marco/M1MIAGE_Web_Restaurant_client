<template>
  <v-card>
      <v-card-title>
          {{ restaurant.name }}
          <v-spacer/>
          <v-menu
            bottom
            left
            v-if="showMenu"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>
              <v-list-item>
                <v-list-item-title class="red--text"><v-icon color="red" @click="$emit('delete-restaurant', restaurant)">mdi-delete</v-icon> Delete</v-list-item-title>
              </v-list-item>
              <v-list-item>
                <v-list-item-title class="gray--text"><v-icon color="gray" @click="$emit('edit-restaurant', restaurant)">mdi-pen</v-icon> Edit</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
      </v-card-title>
      <v-card-text :to="urlDetail">
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
        },
        showMenu: {
            type: Boolean,
            required: false,
            default: false
        }
    },
    data: function(){
        return {
            urlDetail: `/RestaurantDetail?id=${this.restaurant._id}`
        }
    },
    mounted(){
        console.log(this.restaurant)
    }
}
</script>

<style>

</style>