<template>
  <v-navigation-drawer dark app permanent>
      <v-list>
          <v-list-item class="title">
              Restaurant
          </v-list-item>
      </v-list>
      <v-divider></v-divider>
      <v-list>
          <v-list-item to="/">
              <v-icon>mdi-home</v-icon> Accueil
          </v-list-item>
      </v-list>
      <v-divider></v-divider>
        <v-list>
            <v-list-item v-for="item in actions" :key="item.name">
                <div class="item-drawer" @click="item.action"><v-icon>{{ item.icon }}</v-icon> {{ item.name }}</div>
            </v-list-item>
        </v-list>
      <v-divider/>
      <v-list>
          <v-list-item v-for="(r, i) in reservations" :key="i">
              <v-row>
                  <v-col cols="8">
                      <v-row>
                          <v-col cols="12">{{ r.date }} {{ r.time }}</v-col>
                          <v-col cols="12">Restaurant <i>{{ r.restaurant.name }}</i></v-col>
                      </v-row>
                  </v-col>
                  <v-col cols="4"><v-btn icon @click="deleteReservation(r, i)"><v-icon color="red">mdi-trash-can-outline</v-icon></v-btn></v-col>
              </v-row>
            <p></p>
            <p></p>
          </v-list-item>
      </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {
    data: function(){
        return {
            actions: [
                {icon: 'mdi-plus', name: 'Ajouter un nouveau restaurant', action: this.displayPopup},
            ],
            reservations: []
        }
    },
    methods: {
        displayPopup(){
            this.$emit('displayPopupCreateRestaurant')
        },
        deleteReservation(reservation, index){
            let reservations = JSON.parse(localStorage.getItem('reservation'))
            reservations.splice(index, 1)
            this.reservations.splice(index, 1)
            localStorage.setItem('reservation', JSON.stringify(reservations))

        }
    },
    mounted(){
        let reservations = localStorage.getItem('reservation')
        if(reservations != null)
            this.reservations = JSON.parse(reservations)

    }
}
</script>

<style>
    .item-drawer{
        transition: background-color 0.25s;
        border-radius: 5px;
        padding: 5px;
        text-align: center;
        cursor: pointer;
    }
    .item-drawer:hover{
        background-color: #000;
        opacity: 0.5;
    }
</style>