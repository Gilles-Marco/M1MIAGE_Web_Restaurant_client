<template>
  <v-dialog v-model="showDialog">
    <v-card>
      <v-card-title>Créer/Éditer un restaurant</v-card-title>
      <v-card-text>
          <v-form
          ref="formRestaurant">
            <v-row>
              <v-col cols="12">
              <v-text-field required v-model="formRestaurant.name" label="Nom du restaurant"></v-text-field>
              </v-col>
              <v-col cols="12">
              <v-text-field required v-model="formRestaurant.cuisine" label="Cuisine du restaurant"></v-text-field>
              </v-col>
              <v-col cols="6">
              <v-text-field required v-model="formRestaurant.borough" label="Ville"></v-text-field>
              </v-col >
              <v-col cols="6">
              <v-text-field required v-model="formRestaurant.address.zipcode" label="Code postal"></v-text-field>
              </v-col>
              <v-col cols="12">
              <v-text-field required v-model="formRestaurant.address.street" label="Rue"></v-text-field>
              </v-col>
              <v-col cols="6">
              <v-text-field required v-model="formRestaurant.address.building" label="Numéro du batiment"></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-row>
                  <v-col cols="6">
                    <v-text-field required v-model="formRestaurant.address.coord[1]" label="Latitude"></v-text-field>
                  </v-col>
                  <v-col cols="6">
                    <v-text-field required v-model="formRestaurant.address.coord[0]" label="Longitude"></v-text-field>
                  </v-col>
                </v-row>
              </v-col>
            </v-row>
            <v-btn color="success" @click="validate" v-show="!formRestaurant._id">Créer un restaurant</v-btn>
            <v-btn color="primary" @click="editer" v-show="formRestaurant._id">Éditer le restaurant</v-btn>
          </v-form>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data: () => {
    return {
      showDialog: false,
      formRestaurant: {
          name: "",
          cuisine: "",
          id: "",
          borough: '',
          address: {
            zipcode: '',
            street : '',
            building: '',
            coord:[0, 0]
          }
      }
    };
  },
  methods: {
    openDialog() {
      this.showDialog = true;
    },
    openDialogEdit(restaurant){
      this.formRestaurant = restaurant
      this.showDialog = true;
    },
    validate(){
      let url = `http://localhost:8080/api/restaurants`
      let formData = this.formRestaurant
      formData.nom = formData.name
      fetch(url, {
        method: 'POST',
        mode: 'cors',
        body: JSON.stringify(formData),
        headers:{
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        }
      }).then((response)=>{
        this.$emit('create-restaurant', formData)
        console.log(response)
        this.showDialog = false
      }).catch((error)=>{
        this.showDialog = false
        console.error(error)
      })
    },
    editer(){
      let data = this.formRestaurant
      this.formRestaurant.nom = this.formRestaurant.name
      fetch(`http://localhost:8080/api/restaurants/${this.formRestaurant._id}`, {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "PUT",
        mode: "cors",
        body: JSON.stringify(data),
      }).then((response) => {
        console.log(response)
        this.$emit('edit-restaurant', this.formRestaurant)
        this.showDialog = false
      }).catch((error)=>{
        console.error(error)
        this.showDialog = false
      })
    }
  },
};
</script>

<style>
</style>