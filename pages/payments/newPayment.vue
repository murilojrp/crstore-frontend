<template>
  <v-container>
    <h1>New Payment Method</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
                <v-text-field
                    v-model="payment.id"
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
                    v-model="payment.method"
                    placeholder="Method"
                    :rules="rule"
                    required
                    label="Method"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/payments"
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
    name: 'NewPaymentPage',
    data () {
        return {
          valid: false,
          payment: {
              id: null,
              method: null
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
      },
  
    methods: {
        async persist () {
            try {
                //primeiro valida-se se o formulário está preenchido
                if (!this.valid) {
                    return this.$toast.warning('The registration form is not valid!')
                }
        //montamos a variárel categoria para enviar nos posts
                let payment = {
                method: this.payment.method
            }
        //caso não tenha ID na tela, significa que é um cadastro NOVO
        //por isso ele vai apenas com o objeto da categoria para o cadastro
        //como no final tem um RETURN, ele vai cair fora da função PERSISTIR
        if (!this.payment.id) {
          await this.$axios.$post('http://localhost:3333/payments', payment);
          this.$toast.success('Registration successfully completed!');
          return this.$router.push('/payments');
        }
        await this.$axios.$post(`http://localhost:3333/payments/${this.payment.id}`, payment);
        this.$toast.success('Registration successfully updated!');
        return this.$router.push('/payments');
      } catch (error) {
        this.$toast.error('An error occurred while registering!');
      }
      },
      async getById (id) {
        this.payment = await this.$axios.$get(`http://localhost:3333/payments/${id}`);
      }
    }
  }
</script>