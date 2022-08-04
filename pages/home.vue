<template>
</template>

<script>
export default {
  name: 'HomePage',
  layout: 'user',

  data () {
      items: []
    },
  
  created () {
        this.getItems();
  },
  
  methods: {
    async getItems () {
        this.items = await this.$axios.$get('http://localhost:3333/items');
    }
  }
}

</script>

<style>

</style>