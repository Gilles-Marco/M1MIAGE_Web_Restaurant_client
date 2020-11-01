<template>
  <v-card class="pa-3" style="width: fit-content;">
    <p class="caption text-gray">Published the {{ beautifiedDate }}</p>
    <p :class="gradeColor">{{ grade.grade }}, {{ grade.score }} points</p>
    <p></p>
  </v-card>
</template>

<script>
export default {
    props:{
        grade: {
            type: Object,
            required: true,
            default: ()=>{return {date: '', grade: '', score: 0}}
        }
    },
    computed: {
        beautifiedDate(){
            let ratedTime = new Date(this.grade.date)
            return `${ratedTime.getMonth()+1}/${ratedTime.getDate()}/${ratedTime.getFullYear()}`
        },
        gradeColor(){
            switch(this.grade.grade){
                case 'A':
                    return "green--text"
                case 'B':
                    return "blue--text"
                case 'C':
                    return 'yellow--text'
                default: 
                    return "black--text"
            }
        }
    }
}
</script>

<style>

</style>