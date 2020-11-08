<template>
  <v-dialog v-model="showDialog">
      
  </v-dialog>
</template>

<script>
export default {
    data: function(){
        return {
            showDialog: false,
        }
    },
    methods:{
        openDialog(){
            this.showDialog = true
        },
        closeDialog(){
            this.showDialog = false
        }
    }
}
</script>

<style>

</style>