<template>
  <v-container>
    <h1>New Address</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="cart.item"
                    placeholder="Street"
                    :rules="rule"
                    required
                    label="Street"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="cart.amount"
                    placeholder="Neighborhood"
                    :rules="rule"
                    required
                    label="Neighborhood"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="cart.price"
                    placeholder="Address"
                    :rules="rule"
                    required
                    label="Address"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="cart.number"
                    placeholder="Number"
                    :rules="rule"
                    required
                    label="Number"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="cart.complement"
                    placeholder="Complement"
                    :rules="rule"
                    required
                    label="Complement"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/addresses"
    >
    Cancel
    </v-btn>
    <v-btn
        outlined
        @click="persist"
    >
    Save
    </v-btn>
  </v-container>
</template>

<script>
export default {
    name: 'NewAddressPage',
    layout: 'user',
    data () {
        return {
          valid: false,
          address: {
              id: null,
              street: null,
              neighborhood: null,
              address: null,
              number: null,
              complement:null
          },
          rule: [
              v => !!v || 'Required field'
          ]
        }
    },
      created () {
          if (this.$route?.params?.id) {
          this.getById(this.$route.params.id);
          }
      },
  
    methods: {
        async persist () {
            try {
            if (!this.valid) {
                return this.$toast.warning('The registration form is not valid!')
            }
            let address = {
            address: this.address.address,
            street: this.address.street,
            neighborhood: this.address.neighborhood,
            number: this.address.number,
            complement: this.address.complement
            }
            if (!this.address.id) {
                await this.$api.post('/addresses/persist', address);
                this.$toast.success('Registration successfully completed!');
                return this.$router.push('/addresses');
            }
            await this.$api.post(`/addresses/persist/${this.address.id}`, address);
            this.$toast.success('Registration successfully updated!');
            return this.$router.push('/addresses');
            } catch (error) {
            this.$toast.error('An error occurred while registering!');
            }
      },
      async getById (id) {
        this.address = await this.$api.get(`/addresses/${id}`);
      }
    }
  }
</script>