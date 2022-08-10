<template>
<v-container>
    <v-container
    style="background-color: purple"
    >
      <v-container>
      <h1
      style="margin-left:30%"
      >
        BOAS COMPRAS
      </h1>
      <v-row>
        <v-btn
        style="margin-left:35%; margin-top:5%"
        color="green"
        >
        FAZER NOVO PEDIDO
        </v-btn>
      </v-row>
      <br> <br>
      </v-container>
    <v-row>
      <v-card elevation="24" shaped style="width: 20%; margin-left: 1%;  margin-bottom: 5%; margin-right: 1%; padding-bottom: 1%; background-color: gray; "
      v-for="item in items" 
      :key="item.id">
        <v-img
        :src="item.image"
        height="300px"
        ></v-img>
        <v-card-title>
          {{ item.name }}
        </v-card-title>
        <v-card-subtitle>
          R$ {{ item.price }} | {{ item.category.name }}
        </v-card-subtitle>
        <v-btn
          fab
          small
          color="green"
          style="margin-left: 5%; margin-right:5%; margin-bottom: 5%; margin-top: 5%;"
        >
          <v-icon>
            mdi-plus
          </v-icon>
        </v-btn>
        <v-btn
          fab
          small
          color="red"
          style="margin-left: 5%; margin-right:5%; margin-bottom: 5%; margin-top: 5%;"
        >
          <v-icon>
            mdi-minus
          </v-icon>
        </v-btn>
      </v-card>
    </v-row>
  </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'HomeUserPage',

  layout: 'user',

  data () {
    return {
      items: []
      }
    },
  
  created () {
    this.getItems();
  },
  
  methods: {
    async getItems () {
      let response = await this.$api.get('/items');
      this.items = response.data;
    }
  }
}

</script>
