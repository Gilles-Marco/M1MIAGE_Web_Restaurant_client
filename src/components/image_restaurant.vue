<template>
  <v-img :src="urlImage" :height="height" :width="width"></v-img>
</template>

<script>

export default {
    props: {
        height: {
            type: String,
            default: "200px"
        },
        width: {
            type: String,
            default: "200px"
        }
    },
    computed: {
        urlImage(){
            let index = Math.ceil(Math.random() * (15-1))
            return `${index}.jpg`
        }
    }
}
</script>

<style>

</style>