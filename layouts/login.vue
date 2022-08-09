<template>
  <v-app dark>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ `${new Date().getFullYear()} | CRStore ` }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'LoginLayout',
  data () {
    return {
      title: 'CRStore'
    }
  }
}
</script>
