<template>
  <div>
      <div class="font-italic font-weight-bold">{{ title }}</div>
      <div>{{ content }}</div>
  </div>
</template>

<script>
export default {
    props:{
        title: {
            type: String,
            value: ""
        },
        content:{
            type: String,
            value: ""
        }
    }
}
</script>

<style>

</style>