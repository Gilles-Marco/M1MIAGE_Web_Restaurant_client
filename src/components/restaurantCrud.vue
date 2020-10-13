<template>
  <v-dialog v-model="showDialog">
    <v-card>
      <v-card-title>Créer/Éditer un restaurant</v-card-title>
      <v-card-text>
          <v-form
          ref="formRestaurant">
            <v-text-field required v-model="formRestaurant.name" label="Nom du restaurant"></v-text-field>
            <v-text-field required v-model="formRestaurant.cuisine" label="Cuisine du restaurant"></v-text-field>
            <v-btn color="success" @click="validate" v-show="!formRestaurant.id">Créer un restaurant</v-btn>
            <v-btn color="primary" @click="editer" v-show="formRestaurant.id">Éditer le restaurant</v-btn>
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
      }
    };
  },
  methods: {
    openDialog() {
      this.showDialog = true;
    },
    validate(){

    },
    editer(){

    }
  },
};
</script>

<style>
</style>