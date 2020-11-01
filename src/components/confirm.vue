<template>
  <v-dialog v-model="showDialog">
    <v-card>
      <v-card-title> {{ title }}</v-card-title>
      <v-card-text>
            {{ message }}
      </v-card-text>
      <v-card-actions>
          <v-btn @click="cancelButtonAction">{{ cancelButtonText }}</v-btn>
          <v-btn :color="confirmColor" :class="textColor" @click="confirmAction">{{ confirmButtonText }}</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
    props:{
        title: {
            type: String,
            required: false,
            default: ''
        },
        message: {
            type: String,
            required: false,
            default: '',
        },
        confirmColor: {
            type: String,
            required: false,
            default: "red"
        },
        confirmTextColor: {
            type: String,
            required: false,
            default: 'white'
        },
        cancelButtonText: {
            type: String,
            required: false,
            default: 'Cancel'
        },
        cancelButtonAction: {
            type: Function,
            required: false,
            default: function(){
                return function(){this.showDialog = false}
            }
        },
        confirmButtonText: {
            type: String,
            required: false,
            default: 'Confirm'
        },
        confirmButtonAction: {
            type: Function,
            required: false,
            defaut: function(){
                return function(){}
            }
        },
        args: {
            type: Array,
            required: false,
            default: function(){return []}
        }
    },
    data: function(){
        return {
            showDialog: false,
        }
    },
    methods: {
        openDialog() {
            this.showDialog = true;
        },
        confirmAction(){
            this.confirmButtonAction(this.args).then(()=>{
                this.showDialog = false
            }).catch((error)=>{
                console.error(error)
                this.showDialog = false
            })
        }
    },
    computed:{
        textColor(){
            return `${this.confirmTextColor}--text`
        }
    }
}
</script>

<style>

</style>