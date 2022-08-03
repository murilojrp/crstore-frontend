<template>
  <v-container>
    <h1>New Item</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
                <v-text-field
                    v-model="item.id"
                    placeholder="Code"
                    label="Code"
                    disabled
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="item.name"
                    placeholder="Name"
                    :rules="rule"
                    required
                    label="Name"
                    outlined
                >
                </v-text-field>
            </v-col>
            <v-col>
              <v-text-field
                v-model="item.price"
                :rules="rule"
                aria-required="outlined"
                label="Price"
              ></v-text-field>
            </v-col>
            <v-col>
                <v-autocomplete
              v-model="item.idCategory"
              :items="category"
              :rules="rule"
              required
              outlined
              label="Category"
              item-text="name"
              item-value="id"
            ></v-autocomplete>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-col>
      <v-btn
          outlined
          to="/items"
      >
      Cancel
      </v-btn>
      <v-btn
          outlined
          @click="persist"
      >
      Save
      </v-btn>
    </v-col>
  </v-container>
</template>

<script>
export default {
    name: 'NewItemPage',
    data () {
        return {
          valid: false,
          item: {
              id: null,
              name: null,
              price: null,
              idCategory: null
          },
          category: {
            name: null
          },
          rule: [
              v => !!v || 'Required field'
          ]
        }
    },
      created () {

          if (this.$route?.params?.id) {
          this.getById(this.$route.params.id)
          }

        this.getCategories();
      },
  
    methods: {
      async persist () {

        try {

          if (!this.valid) {
            return this.$toast.warning('The registration form is not valid!')
          }
          let item = {
            name: this.item.name,
            price: this.item.price,
            idCategory: this.item.idCategory
          }

        if (!this.item.id) {
          await this.$axios.$post('http://localhost:3333/items', item);
          this.$toast.success('Registration successfully completed!');
          return this.$router.push('/items');
        }
          await this.$axios.$post(`http://localhost:3333/items/${this.item.id}`, item);
          this.$toast.success('Registration successfully updated!');
          return this.$router.push('/items');
        } 

        catch (error) {
          this.$toast.error('An error occurred while registering!');
        }
      },

      async getById (id) {
        this.item = await this.$axios.$get(`http://localhost:3333/items/${id}`);
      },

      async getCategories () {
        this.category = await this.$axios.$get(`http://localhost:3333/categories`);
      }
    }
  }
</script>