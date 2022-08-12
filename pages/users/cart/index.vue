<template>
  <v-container>
    <h1>Meu Carrinho</h1>
    <hr>
    <br>
    <v-container>
        <v-row
            v-for="item in carts" 
            :key="item.id"
        >
        <v-card elevation="24" shaped style="width: 30%; margin-left: 1%;  margin-bottom: 5%; margin-right: 1%; padding-bottom: 1%; background-color: gray; "
            v-for="item in carts" 
            :key="item.id"
        >
            <v-img
                :src="carts[0].item.image"
                height="300px"
            ></v-img>
            <v-card-title>
                {{ carts[0].item.name }}
            </v-card-title>
            <v-card-subtitle>
                R$ {{ carts[0].item.price }} | {{ carts.amount }}
            </v-card-subtitle>
            <v-btn
          style="background-color:blue; margin-left: 5%;  margin-right: 5%; bottom: 2%;"
          outlined
          @click="editItem()"
        >
          EDITAR
        </v-btn>
        <v-btn
          style="background-color:red; margin-left: 5%;  margin-right: 5%; bottom: 2%;"
          outlined
          @click="deleteItem()"
        >
          REMOVER
        </v-btn>
        </v-card>
        </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
    name: 'CartPage',
    layout: 'user',
    data () {
        return {
            carts: []
        }
    },
    created () {
        this.getCart()
    },
    methods: {
        async getCart () {
            let response = await this.$api.$get('/cart');
            console.log(response);
            console.log("fora do for");
            let items = [{}];
            for (let index = 0; index < response.length; index++) {
                console.log("dentro do for");
                this.items = response[index];
                for (let index = 0; index < items.length; index++) {
                    this.carts = items[index]
                console.log(this.carts);
                }
            }
            console.log("fim da funcao");
        },
        async deleteItem (carts) {
            try {
                let response = await this.$api.post('/cart/destroy', { id: carts.id });
                this.$toast.success(`Address successfully deleted!`);
                this.getCart ();
            } catch (error) {
            this.$toast.error('An error occurred while fulfilling the request. Contact the ADM.')
        }
     },
        async editItem (carts) {
        this.$router.push({
            name: 'users-editCart',
            params: { id: carts.id }
        });
        }
    }
}
</script>