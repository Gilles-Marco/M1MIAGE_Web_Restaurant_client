<template>
  <v-dialog v-model="showDialog">
      <v-card>
      <v-card-title>Recherche avancée</v-card-title>
      <v-card-text>
          <v-form ref="formSearch">
            <v-row>
              <v-col cols="12">
                <v-text-field v-model="restaurant.name" label="Nom du restaurant"/>
              </v-col>
              <v-col cols="12">
                <v-text-field v-model="restaurant.cuisine" label="Cuisine du restaurant"/>
              </v-col>
              <v-col cols="4">
                <v-text-field v-model="restaurant.borough" label="Ville du restaurant"/>
              </v-col>
              <v-col cols="4">
                <v-text-field v-model="restaurant.address.zipcode" label="Code postal du restaurant"/>
              </v-col>
              <v-col cols="4">
                <v-text-field v-model="restaurant.address.building" label="Numéro de bâtiment"/>
              </v-col>
              <v-col cols="9">
                <v-text-field v-model="restaurant.address.street" label="Rue du restaurant"/>
              </v-col>
            </v-row>
          </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn color="red" class="white--text" @click="closeDialog()">Annuler</v-btn>
        <v-btn color='primary' @click="search()">Rechercher</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data: () => {
    return {
      showDialog: false,
      restaurant: {
        name: '',
        borough: '',
        cuisine: '',
        address: {
          zipcode: '',
          building: '',
          street: '',
        },
      }
    };
  },
  methods: {
    openDialog() {
      this.showDialog = true;
    },
    closeDialog(){
      this.restaurant.name = ''
      this.restaurant.borough = ''
      this.restaurant.cuisine = ''
      this.restaurant.address.zipcode = ''
      this.restaurant.address.building = ''
      this.restaurant.address.street = ''
      this.showDialog = false
    },
    search(){
      this.getRestaurantCount().then((count)=>{
        fetch(`http://localhost:8080/api/restaurants?page=1&pagesize=${count}`).then((response)=>{
          response.json().then((value)=>{
            let data = value.data
            let filteredRestaurant = data.filter((r) => this.matchFields(r))
            this.$emit('advanced-search-restaurants', filteredRestaurant)
            this.closeDialog()
          })
        })
      })
    },
    getRestaurantCount(){
      return new Promise((resolve, reject)=>{
        fetch('http://localhost:8080/api/restaurants/count').then((response)=>{
          response.json().then((value)=>{
            resolve(value)
          })
        }).catch((error)=>{
          reject(error)
        })
      })
    },
    matchFields(restaurant){
      let keys = Object.keys(restaurant)
      let match = true
      let exclude_fields = ['grades']

      for(let key of keys){
        if ( !match )
          break;
        else if (exclude_fields.includes(key));
        else if( key == 'address' ) {
          let keysAddress = Object.keys(restaurant.address)
          for(let ka of keysAddress){
            if( ka != 'coord'){
              if ( !restaurant.address[ka].match(this.restaurant.address[ka]))
                match = false;
                break;
            }
          }
        }
        else{
          try{
            if (restaurant[key] == null && this.restaurant[key] != ''){
              match = false;
            }
            else if(restaurant[key] == null);
            else if (!restaurant[key].match(this.restaurant[key])){
              match = false;
            }
          }
          catch {
            console.error(`This key provoqued an error ${key}, value was = ${restaurant[key]} and form was ${this.restaurant[key]}`)
          }
        }
      }

      if(match)
        return restaurant
    }
  }
};
</script>

<style>

</style>