<template>
  <view-with-drawer>
      <v-container>
        <menu-detail :restaurant="restaurant" ref="menu_detail"/>
        <v-row>
            <v-col cols="4">
                <v-row>
                    <v-col cols="12"><h1>{{ restaurant.name }}</h1></v-col>
                    <v-col cols="12">
                        <h2>Cuisine 🍽</h2>
                        <p class="subtitle-1">{{ restaurant.cuisine }}</p>
                        <v-btn @click="showMenu" color="orange" class="white--text">Voir le menu</v-btn>
                    </v-col>
                </v-row>
            </v-col>
            <v-col cols="8">
                <image-restaurant width="340px" height="200px"/>
            </v-col>
            <v-col cols="12"><h2>Adresse</h2>{{ restaurantAddress }}</v-col>
            <v-col cols="12">
                <iframe
                    width="600"
                    height="450"
                    frameborder="0" style="border:0"
                    :src="mapsUrl" allowfullscreen>
                </iframe>
            </v-col>
            <v-col>
                <h2>Notes et commentaires</h2>
                <v-row>
                    <v-col md="3" lg="2" v-for="grade in restaurant.grades" :key="grade.date" >
                        <grades :grade="grade"/>
                    </v-col>
                </v-row>
            </v-col>
        </v-row>
      </v-container>
  </view-with-drawer>
</template>

<script>
import ViewWithDrawer from '@/layout/view_with_drawer.vue'
import Grades from '@/components/grades.vue'
import ImageRestaurant from '@/components/image_restaurant.vue'
import MenuDetail from '@/components/menuDetail.vue'

export default {
    components:{
        ViewWithDrawer,
        Grades,
        ImageRestaurant,
        MenuDetail
    },
    data: function(){
        return {
            restaurant: {},
            restaurantAddress: '',
            mapsUrl: '',
            api_key: '',
            restaurant_type: '',
            TYPE: ['bistrot', 'gastronomique']
        }
    },
    mounted(){
        // Fetch the restaurant id
        this.api_key = process.env.VUE_APP_MAPS_API_KEY
        let restaurant_id = this.$route.query.id
        fetch(`http://localhost:8080/api/restaurants/${restaurant_id}`).then((response)=>{
            response.json().then((value)=>{
                this.restaurant = value.restaurant
                this.restaurantAddress = `${this.restaurant.borough} ${this.restaurant.address.zipcode}, Building ${this.restaurant.address.building} on ${this.restaurant.address.street}`
                this.mapsUrl = `https://www.google.com/maps/embed/v1/search?key=${this.api_key}&q=${this.restaurant.address.building}+${this.restaurant.address.street},${this.restaurant.borough}+${this.restaurant.address.zipcode}`
            }).catch((error)=>{
                console.error(error)
            })
        }).catch((error)=>{
            console.error(error)
        })
    },
    methods: {
        showMenu(){
            this.$refs.menu_detail.openDialog()
        }
    }
}
</script>

<style>

</style>