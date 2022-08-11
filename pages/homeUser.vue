<template>
<v-container style="background-color: lightgray">
    <v-container>
      <v-container>
      <h1
      style="margin-left:30%; color: black"
      >
        BOAS COMPRAS
      </h1> <br> <br> <br>
      </v-container>
    <v-row>
      <v-card elevation="24" shaped style="width: 30%; margin-left: 1%;  margin-bottom: 5%; margin-right: 1%; padding-bottom: 1%; background-color: gray; "
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
        <v-text-field
          v-model="item.amount"
          placeholder=0
          label="Quantidade"
          outlined
          style="width:50%; margin-left: 20%;  margin-right: 5%; margin-bottom: 10%; margin-top: 1%;"
        >
        </v-text-field>
        <v-btn
          style="background-color:green; margin-left: 5%;  margin-right: 5%; position:absolute; bottom: 2%;"
          @click="addItemInCart(item.id, item.price, item.amount)"
        >
          ADICIONAR AO CARRINHO
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
      items: [],
      cart: {
        idItem: null,
        amount: null,
        price: null
      },
      // itemsAlreadyOnCart: []
      }
    },
  
  created () {
    this.getItems();
  },
  
  methods: {
    async getItems () {
      let response = await this.$api.get('/items');
      this.items = response.data;
    },
    async addItemInCart(itemId, itemPrice, itemAmount) {
      this.cart.amount = itemAmount
      // this.itemsAlreadyOnCart.push(itemId);
      // if (this.cart.idItem == null || !this.itemsAlreadyOnCart.includes(this.cart.idItem)) {
      this.cart.idItem = itemId;
      this.cart.price = itemPrice;
      console.log(this.cart);
      let response = await this.$api.post('/cart', this.cart);
      this.$toast.success('Produto adicionado ao carrinho!')
      // }
    }
  }
}

</script>
