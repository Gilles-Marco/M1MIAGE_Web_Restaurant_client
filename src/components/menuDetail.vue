<template>
  <v-dialog v-model="showDialog">
      <v-container class="white">
        <h1>Menu {{ type }}</h1>
        <v-row>
            <v-col cols="12">
                <v-row>
                    <v-col cols="6"><h1></h1></v-col>
                    <v-col cols="4"></v-col>
                    <v-col cols="2"><p></p></v-col>
                </v-row>
                <v-row>
                    <v-col cols="8">
                        <v-row>
                            <v-col cols="12"><h1>{{generatedMenu.hors_doeuvre.title}}</h1></v-col>
                            <v-col cols="12">{{ generatedMenu.hors_doeuvre.price }} €</v-col>
                        </v-row>
                    </v-col>
                    <v-col cols="4">
                        <v-img :src="generatedMenu.hors_doeuvre.image" />
                    </v-col>
                </v-row>
            </v-col>
            <v-col cols="12">
                <v-row>
                    <v-col cols="8">
                        <v-row>
                            <v-col cols="12"><h1>{{generatedMenu.plat_du_jour.title}}</h1></v-col>
                            <v-col cols="12">{{ generatedMenu.plat_du_jour.price }} €</v-col>
                        </v-row>
                    </v-col>
                    <v-col cols="4">
                        <v-img :src="generatedMenu.plat_du_jour.image" />
                    </v-col>
                </v-row>
            </v-col>
            <v-col cols="12">
                <v-row>
                    <v-col cols="8">
                        <v-row>
                            <v-col cols="12"><h1>{{generatedMenu.dessert.title}}</h1></v-col>
                            <v-col cols="12">{{ generatedMenu.dessert.price }} €</v-col>
                        </v-row>
                    </v-col>
                    <v-col cols="4"><v-img :src="generatedMenu.dessert.image" /></v-col>
                </v-row>
            </v-col>
            <v-col>
                <h2>Prix total {{ generatedMenu.hors_doeuvre.price+generatedMenu.plat_du_jour.price+generatedMenu.dessert.price }} €</h2>
            </v-col>
        </v-row>
      </v-container>
  </v-dialog>
</template>

<script>
import menus from '../../public/menus.json'

export default {
    props: {
        type: {
            type: String,
            default: function(){
                let choix = ['Plat midi', 'Gastronomique']
                return choix[Math.floor(Math.random() * choix.length)]
            }
        }
    },
    data: function(){
        return {
            showDialog: false
        }
    },
    computed: {
        generatedMenu(){
            let filteredMenu = menus.filter(item => item.menu == this.type)
            let horsdoeuvre = filteredMenu.filter(item => item.type == "hors d'oeuvre")
            let platdujour = filteredMenu.filter(item => item.type == 'Plat du jour')
            let dessert = filteredMenu.filter(item => item.type == 'Dessert')
            let h = horsdoeuvre[Math.floor(Math.random() * horsdoeuvre.length)]
            let p = platdujour[Math.floor(Math.random() * platdujour.length)]
            let d = dessert[Math.floor(Math.random() * dessert.length)]
            let menu = {
                'hors_doeuvre': h,
                'plat_du_jour': p,
                'dessert': d
            }
            return menu
        }
    },
    methods: {
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