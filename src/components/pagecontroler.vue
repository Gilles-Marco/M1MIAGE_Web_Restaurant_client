<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <div>
            <v-btn color="primary" class="mx-1" @click="previousPage">Page précédente</v-btn>
            <v-btn color="primary" class="mx-1" @click="nextPage">Page suivante</v-btn>
            <span class="subtitle mx-1">Page {{ page }}</span>
        </div>
      </v-col>
      <v-col cols="12">
        <input
          type="range"
          min="5"
          step="5"
          max="100"
          v-model="pagesize"
        />
        {{ pagesize }} Restaurants par page
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
      value: {
          type: Object,
          required: true,
          default: ()=>{
              return {page:1, pagesize:10}
          }
      }
  },
  data: function () {
    return {
      page: 1,
      pagesize: 10,
    };
  },
  methods: {
    nextPage() {
      this.page++;
      this.$emit("input", { page: this.page, pagesize: this.pagesize });
    },
    previousPage() {
      this.page--;
      this.page = this.page < 0 ? 0 : this.page;
      this.$emit("input", { page: this.page, pagesize: this.pagesize });
    },
  },
  watch: {
      pagesize(){
          this.$emit('input', {page: this.page, pagesize: this.pagesize})
      }
  }
};
</script>

<style>
</style>