<template>
  <v-container>
    <v-table>
      <v-row>
        <v-col>
          <v-container style="margin-top:25%; font-size:50px; margin-left:100px">
            <h1 style="font-size:50px;">
              Bem-Vindo
              <br>
              Faça seu login CRStore!
            </h1>
              <h3 style="font-size:25%">
                Ainda não tem cadastro? Faça um por <a href="/users/register">aqui!</a>
              </h3>
          </v-container>
        </v-col>
        <v-col>
          <v-form style=" border:medium" v-model="valid">
            <v-container style="width:75%; margin-left: 5%; border-radius: 1%; background-color: #202024; margin-top:100px; margin-right:200px; font-size: 25px; padding-top: 50px; padding-left: 50px; padding-right: 50px;padding-bottom: 50px;">
              <v-text-field
                v-model="user.username"
                outlined
                :rules="rule"
                required
                placeholder="Username"
                prepend-inner-icon="mdi-account"
                color="#593e99"
                background-color="#121214"
              />
              <v-text-field
                v-model="user.password"
                outlined
                :rules="rule"
                required
                placeholder="Password"
                prepend-inner-icon="mdi-lock"
                color="#593e99"
                background-color="#121214"
                :append-icon="show1 ? 'mdi-eye-off' : 'mdi-eye'"
                :type="show1 ? 'text' : 'password'"
                @click:append="show1 = !show1"
              />
              <h3
                style="font-size:small; color:#6e4cbf; margin-top:-7%"
              >
                Esqueci minha senha
              </h3>
              <v-btn
                style="background-color: #41356b; margin-top: 5%; margin-left: 35%;"
                @click="login"
              >
                ENTRAR 
              </v-btn>
            </v-container>
          </v-form>
        </v-col>
      </v-row>
    </v-table>
  </v-container>
</template>

<script>
export default {
  layout: 'login',
  name: 'IndexPage',

  data() {
    return {
      show1: false,
      valid: false,
      user: {
        username: null,
        password: null
      },
      rule: [
        v => !!v || 'Required field'
      ]
    }
  },

  methods: {
    async login () {
      
      if (!this.valid) {
        return this.$toast.warning('The registration form is not valid!')
      } else {
      let response = await this.$axios.$post('http://localhost:3333/users/login', this.user);

        if (!response.token) {
          return this.$toast.info('Username or password invalid!')
        } else {
        this.$toast.success('Welcome back!')
        localStorage.setItem('crstore-api-token', response.token);
        this.validate(response.role);
      }
    }
    },
    async validate (role) {
      if (role == "customer") {
        this.$router.push('/homeUser');
      } else {
        this.$router.push('/homeAdmin');
      }
    }
  }
}
</script>