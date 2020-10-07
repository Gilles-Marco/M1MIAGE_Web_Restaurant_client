<template>
    <v-card v-show="showDialog">
        <v-card-title>Créer/Éditer un restaurant</v-card-title>
        <v-card-text>
            Hello
        </v-card-text>
    </v-card>
</template>

<script>
export default {
    data: ()=>{
        return {
            showDialog: false
        }
    },
    methods: {
        openDialog(){
            this.showDialog = true;
        }
    }
}
</script>

<style>

</style>