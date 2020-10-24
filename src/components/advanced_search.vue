<template>
  <v-dialog>
      <v-card>
      <v-card-title>Advanced Search</v-card-title>
      <v-card-text>
          <v-form ref="formSearch">
            
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