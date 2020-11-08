<template>
  <v-dialog v-model="showDialog">
      <v-card>
        <v-card-title>
            <v-tabs v-model="tab">
                <v-tab>
                    Menu du midi
                </v-tab>
                <v-tab>
                    Menu gastronomique
                </v-tab>
            </v-tabs>
        </v-card-title>
        <v-card-text>
            <v-tabs-items v-model="tab">
                <v-tab-item>
                    <v-expansion-panels>
                        <v-expansion-panel>
                            <v-expansion-panel-header>Entrée</v-expansion-panel-header>
                            <v-expansion-panel-content>
                                <v-row v-for="(item, i) in midiEntree" :key="i">
                                    <v-col cols="2"><v-img :src="item.image"/></v-col>
                                    <v-col cols="7"><h1>{{item.title}}</h1></v-col>
                                    <v-col cols="3"><v-btn color="primary" @click="addToCommand(item)">Ajouter à la commande</v-btn></v-col>
                                </v-row>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                        <v-expansion-panel>
                            <v-expansion-panel-header>Plat</v-expansion-panel-header>
                            <v-expansion-panel-content>
                                <v-row v-for="(item, i) in midiPlat" :key="i">
                                    <v-col cols="2"><v-img :src="item.image"/></v-col>
                                    <v-col cols="7"><h1>{{item.title}}</h1></v-col>
                                    <v-col cols="3"><v-btn color="primary" @click="addToCommand(item)">Ajouter à la commande</v-btn></v-col>
                                </v-row>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                        <v-expansion-panel>
                            <v-expansion-panel-header>Dessert</v-expansion-panel-header>
                            <v-expansion-panel-content>
                                <v-row v-for="(item, i) in midiDessert" :key="i">
                                    <v-col cols="2"><v-img :src="item.image"/></v-col>
                                    <v-col cols="7"><h1>{{item.title}}</h1></v-col>
                                    <v-col cols="3"><v-btn color="primary" @click="addToCommand(item)">Ajouter à la commande</v-btn></v-col>
                                </v-row>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-expansion-panels>
                </v-tab-item>
                <v-tab-item>
                    <v-expansion-panels>
                        <v-expansion-panel>
                            <v-expansion-panel-header>Entrée</v-expansion-panel-header>
                            <v-expansion-panel-content>
                                <v-row v-for="(item, i) in gastronomiqueEntree" :key="i">
                                    <v-col cols="2"><v-img :src="item.image"/></v-col>
                                    <v-col cols="7"><h1>{{item.title}}</h1></v-col>
                                    <v-col cols="3"><v-btn color="primary" @click="addToCommand(item)">Ajouter à la commande</v-btn></v-col>
                                </v-row>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                        <v-expansion-panel>
                            <v-expansion-panel-header>Plat</v-expansion-panel-header>
                            <v-expansion-panel-content>
                                <v-row v-for="(item, i) in gastronomiquePlat" :key="i">
                                    <v-col cols="2"><v-img :src="item.image"/></v-col>
                                    <v-col cols="7"><h1>{{item.title}}</h1></v-col>
                                    <v-col cols="3"><v-btn color="primary" @click="addToCommand(item)">Ajouter à la commande</v-btn></v-col>
                                </v-row>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                        <v-expansion-panel>
                            <v-expansion-panel-header>Dessert</v-expansion-panel-header>
                            <v-expansion-panel-content>
                                <v-row v-for="(item, i) in gastronomiqueDessert" :key="i">
                                    <v-col cols="2"><v-img :src="item.image"/></v-col>
                                    <v-col cols="7"><h1>{{item.title}}</h1></v-col>
                                    <v-col cols="3"><v-btn color="primary" @click="addToCommand(item)">Ajouter à la commande</v-btn></v-col>
                                </v-row>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-expansion-panels>
                </v-tab-item>
            </v-tabs-items>
            <v-card flat v-show="command.length>0">
                <v-card-title><h1>Résumé de votre commande</h1></v-card-title>
                <v-card-text>
                    <v-row v-for="(item, i) in command" :key="i">
                        <v-col cols="3"><h2>{{ item.item.title }}</h2></v-col>
                        <v-col cols="2">{{ item.item.price * item.quantity }} €</v-col>
                        <v-col cols="2">({{ item.item.price}} €/unité)</v-col>
                        <v-col cols="3"><v-text-field type="number" v-model="item.quantity" label="Quantité"/></v-col>
                        <v-col cols="2"><v-btn icon @click="removeFromCommand(item)"><v-icon color="red">mdi-trash-can-outline</v-icon></v-btn></v-col>
                    </v-row>
                    <h3>Total {{ sumMenu }} €</h3>
                </v-card-text>
            </v-card>
            <v-text-field type="date" v-model="dateReservation" label="Date de la réservation"/>
            <v-text-field type="time" v-model="timeReservation" label="Heure de la réservation"/>
            <v-btn :disabled="command.length<1" color="green" class="mt-4 white--text" @click="addReservation">Reservez</v-btn>
        </v-card-text>
      </v-card>
      
  </v-dialog>
</template>

<script>
import menus from '../../public/menus.json'

export default {
    props: {
        restaurant: {
            type: Object,
            default: function(){
                return {}
            }
        }
    },
    data: function(){
        return {
            showDialog: false,
            tab: null,
            command: [],
            dateReservation: `${new Date(Date.now() + 1000*30*60).getFullYear()}-${(new Date(Date.now() + 1000*30*60).getMonth()+1).toString().padStart(2, "0")}-${(new Date(Date.now() + 1000*30*60).getDate()).toString().padStart(2, "0")}`,
            timeReservation: `${new Date(Date.now() + 1000*30*60).getHours().toString().padStart(2, "0")}:${new Date(Date.now() + 1000*30*60).getMinutes().toString().padStart(2, "0")}`
        }
    },
    computed: {
        midiEntree(){
            return menus.filter(item => item.menu == 'Plat midi' & item.type== "hors d'oeuvre")
        },
        midiPlat(){
            return menus.filter(item => item.menu == 'Plat midi' & item.type == "Plat du jour")
        },
        midiDessert(){
            return menus.filter(item => item.menu == 'Plat midi' & item.type == "Dessert")
        },
        gastronomiqueEntree(){
            return menus.filter(item => item.menu == "Gastronomique" & item.type == "hors d'oeuvre")
        },
        gastronomiquePlat(){
            return menus.filter(item => item.menu == 'Gastronomique' & item.type == "Plat du jour")
        },
        gastronomiqueDessert(){
            return menus.filter(item => item.menu == 'Gastronomique' & item.type == "Dessert")
        },
        sumMenu(){
            let total = 0
            for(let item of this.command){
                total += item.quantity * item.item.price
            }
            return total
        }
    },
    methods: {
        openDialog(){
            this.showDialog = true
        },
        closeDialog(){
            this.showDialog = false
        },
        addToCommand(item){
            let index = this.command.findIndex(v => v.item.title == item.title)
            if(index != -1){
                this.command[index].quantity += 1
            }
            else{
                this.command.push({
                    quantity: 1,
                    item: item
                })
            }
        },
        removeFromCommand(item){
            let index = this.command.findIndex(v => v.item.title == item.item.title)
            this.command.splice(index, 1)
        },
        addReservation(){
            let reservation = localStorage.getItem('reservation')
            if(reservation == null)
                reservation = []
            else
                reservation = JSON.parse(reservation)
            reservation.push({date: this.dateReservation, time: this.timeReservation, command: this.command, restaurant: this.restaurant})
            localStorage.setItem('reservation', JSON.stringify(reservation))
            this.showDialog = false
            this.command = []
            this.$router.push('/')
        }
    }
}
</script>