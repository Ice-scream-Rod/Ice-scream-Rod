<template>
  <v-app dark>
    <v-navigation-drawer
      temporary
      v-model="sideNav"
      v-if="userIsAuthenticated">
      <v-list>
        <v-list-tile v-for="(item, i) in menuItems" :key="i" :to="item.link" :class="item.class">
          <v-list-tile-action>
            <v-icon dark>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile> 
        <v-list-tile @click="onLogout" class="hidden-sm-and-up">
          <v-list-tile-action>
            <v-icon dark>exit_to_app</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>Sair</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>       
      </v-list>
    </v-navigation-drawer>
    <v-toolbar fixed v-if="userIsAuthenticated">
      <v-btn icon dark @click.stop="sideNav = !sideNav">
        <v-icon>menu</v-icon>
      </v-btn>
      <v-toolbar-title>
        <router-link to="/" tag="span" style="cursor: pointer;">
          <img src="../static/logo-horizontal.png" alt="Ice Scream" class="mt-2 logo-horizontal">
        </router-link>        
      </v-toolbar-title>  
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn flat to="/profile">
          <v-icon left>face</v-icon>
          Meu Perfil
        </v-btn>
        <v-btn flat @click="onLogout">
          <v-icon left>exit_to_app</v-icon>
          Sair
        </v-btn>
      </v-toolbar-items>    
    </v-toolbar>
    <main>
      <v-container fluid>
        <v-slide-y-transition mode="out-in">
          <router-view></router-view>
        </v-slide-y-transition>
      </v-container>
    </main>
    <v-footer v-show="userIsAuthenticated">
      <v-spacer></v-spacer>
      <span>Desenvolvido por </span>
      <img src="../static/logo_ced.png" alt="CED" height="25px" class="mx-1">
      <b class="primary--text mr-3">CED - Centro de Estudos Dart</b>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        sideNav: false,
        menuItems: [
          { icon: 'supervisor_account', title: 'Usu??rios', link: '/users', class: '' },
          { icon: 'store', title: 'Sorveterias', link: '/shops', class: '' },
          { icon: 'monetization_on', title: 'Devedores', link: '/debtors', class: '' },
          { icon: 'check', title: 'Avalia????es', link: '/reviews', class: '' },
          { icon: 'face', title: 'Meu Perfil', link: '/profile', class: 'hidden-sm-and-up' },
          { icon: 'info', title: 'Sobre', link: '/about' }
        ],
        title: 'Ice Screamer'
      }
    },
    computed: {
      user () {
        return this.$store.getters.user
      },
      userIsAuthenticated () {
        return this.$store.getters.user !== null && this.$store.getters.user !== undefined
      }
    },
    methods: {
      onLogout () {
        this.$store.dispatch('logout')
        this.$router.push('/login')
      }
    }
  }
</script>

<style lang="stylus">
  @import './stylus/main'
  @import './stylus/logo'

  .application > main > .container {
    min-height: 89.9vh;
  }

  .logo-horizontal {
    height: 55px;
  }
</style>
