<template>
  <v-container>
    <h1>Register</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.username"
                    placeholder="Username"
                    :rules="rule"
                    required
                    label="Username"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.name"
                    placeholder="Name"
                    :rules="rule"
                    required
                    label="Name"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.phone"
                    placeholder="Phone"
                    :rules="rule"
                    required
                    label="Phone"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.password"
                    placeholder="Password"
                    :rules="rule"
                    required
                    label="Password"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/index"
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
    name: 'NewCategoryPage',
    data () {
        return {
          valid: false,
          category: {
              id: null,
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
      },
  
    methods: {
        async persist () {
            try {
                //primeiro valida-se se o formulário está preenchido
                if (!this.valid) {
                    return this.$toast.warning('The registration form is not valid!')
                }
        //montamos a variárel categoria para enviar nos posts
                let category = {
                name: this.category.name
            }
        //caso não tenha ID na tela, significa que é um cadastro NOVO
        //por isso ele vai apenas com o objeto da categoria para o cadastro
        //como no final tem um RETURN, ele vai cair fora da função PERSISTIR
        if (!this.category.id) {
          await this.$axios.$post('http://localhost:3333/categories', category);
          this.$toast.success('Registration successfully completed!');
          return this.$router.push('/categories');
        }
        await this.$axios.$post(`http://localhost:3333/categories/${this.category.id}`, category);
        this.$toast.success('Registration successfully updated!');
        return this.$router.push('/categories');
      } catch (error) {
        this.$toast.error('An error occurred while registering!');
      }
      },
      async getById (id) {
        this.category = await this.$axios.$get(`http://localhost:3333/categories/${id}`);
      }
    }
  }
</script>