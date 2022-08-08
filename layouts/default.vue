<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ `${new Date().getFullYear()} | CRStore `}}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Home',
          to: '/homeAdmin'
        },
        {
          icon: 'mdi-list-box-outline',
          title: 'Items',
          to: '/items'
        },
        {
          icon: 'mdi-shape',
          title: 'Categories',
          to: '/categories'
        },
        {
          icon: 'mdi-sale',
          title: 'Discount Coupons',
          to: '/discountCoupons'
        },
        {
          icon: 'mdi-account',
          title: 'Users',
          to: '/users'
        },
        {
          icon: 'mdi-credit-card',
          title: 'Payments',
          to: '/payments'
        },
        {
          icon: 'mdi-map-marker',
          title: 'Addresses',
          to: '/addresses'
        },
        {
          icon: 'mdi-list-box',
          title: 'Orders',
          to: '/orders'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'CRStore'
    }
  }
}
</script>
