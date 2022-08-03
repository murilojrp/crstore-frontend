<template>
  <v-container>
    <v-table>
      <v-row>
        <v-col>
          <v-container style="margin-top:300px; font-size:50px; margin-left:100px">
            <h1 style="font-size:50px;">
              Faça seu login
              <br>
              na plataforma
            </h1>
          </v-container>
        </v-col>
        <v-col>
          <v-form style=" border:medium">
            <v-container style="width:75%; border-radius: 1%; background-color: #202024; margin-top:100px; margin-right:200px; font-size: 25px; padding-top: 50px; padding-left: 50px; padding-right: 50px;padding-bottom: 50px;">
              <v-text-field
                outlined
                placeholder="E-mail"
                prepend-inner-icon="mdi-email"
                color="#593e99"
                background-color="#121214"
              />
              <v-text-field
                outlined
                placeholder="Senha"
                prepend-inner-icon="mdi-lock"
                color="#593e99"
                background-color="#121214"
              />
              <h3
                style="font-size:small; color:#6e4cbf; margin-top:-7%"
                href="/forget"
              >
                Esqueci minha senha
              </h3>
              <v-btn
                style="   background-color: #41356b; margin-top: 5%"
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
  name: 'LoginPage'
}
</script>