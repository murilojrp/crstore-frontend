<template>
  <v-container>
    <h1>New Address</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
                <v-text-field
                    v-model="address.id"
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
                    v-model="address.street"
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
                    v-model="address.neihgborhood"
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
                    v-model="address.address"
                    placeholder="Address"
                    :rules="rule"
                    required
                    label="Address"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/homeUser"
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
    data () {
        return {
          valid: false,
          address: {
              id: null,
              street: null,
              neighborhood: null,
              address: null,
              number: null,
              complement:null,
              idUser: null
          },
          rule: [
              v => !!v || 'Required field'
          ]
        }
    },
      created () {
            this.getUserByToken();
          if (this.$route?.params?.id) {
          this.getById(this.$route.params.id)
          }
      },
  
    methods: {
        async persist () {
            try {
                //primeiro valida-se se o formulário está preenchido
                if (!this.valid) {
                    return this.$toast.warning('The registration form is not valid!')
                }
        //montamos a variárel categoria para enviar nos posts
                let address = {
                street: this.address.street,
                neighborhood: this.address.neighborhood,
                address: this.address.address,
                number: this.address.number,
                complement: this.address.complement,
                idUser: this.address.idUser
            }
        //caso não tenha ID na tela, significa que é um cadastro NOVO
        //por isso ele vai apenas com o objeto da categoria para o cadastro
        //como no final tem um RETURN, ele vai cair fora da função PERSISTIR
        if (!this.address.id) {
          await this.$axios.$post('http://localhost:3333/addresses', address);
          this.$toast.success('Registration successfully completed!');
          return this.$router.push('/homeUser');
        }
        await this.$axios.$post(`http://localhost:3333/addresses/${this.address.id}`, address);
        this.$toast.success('Registration successfully updated!');
        return this.$router.push('/homeUser');
      } catch (error) {
        this.$toast.error('An error occurred while registering!');
      }
      },
      async getById (id) {
        this.address = await this.$axios.$get(`http://localhost:3333/addresses/${id}`);
      }
    }
  }
</script>