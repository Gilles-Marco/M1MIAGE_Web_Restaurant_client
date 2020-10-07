<template>
  <v-navigation-drawer dark app permanent>
      <v-list>
          <v-list-item class="title">
              Restaurant
          </v-list-item>
      </v-list>
      <v-divider></v-divider>
        <v-list>
            <v-list-item v-for="item in actions" :key="item.name">
                <div class="item-drawer" @click="item.action"><v-icon>{{ item.icon }}</v-icon> {{ item.name }}</div>
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
            ]
        }
    },
    methods: {
        displayPopup(){
            this.$emit('displayPopupCreateRestaurant')
        }
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