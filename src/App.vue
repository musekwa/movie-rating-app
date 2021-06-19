<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      fixed
    >
      <v-list dense>
        <router-link v-bind:to="{name: 'Home' }"
          class="side_bar_link">
          <v-list-item>
            <v-list-item-action>
              <v-icon>home</v-icon>
            </v-list-item-action>
            <v-list-item-content>MSK-Tech</v-list-item-content>
          </v-list-item>
        </router-link>
        <router-link v-bind:to="{name: 'Contact' }"
          class="side_bar_link">
          <v-list-item>
            <v-list-item-action>
              <v-icon>contact_mail</v-icon>
            </v-list-item-action>
            <v-list-item-content>Contact</v-list-item-content>
          </v-list-item>
        </router-link>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar color="indigo" dark fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>MSK-Tech</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="d-flex">
        <v-btn id="add_movie_link" text v-bind:to="{ name: 'AddMovie' }"
          v-if="current_user">
          Add Movie
        </v-btn>
        <v-btn id="user_email" text v-if="current_user">
          {{ current_user.fullname.includes(' ')
              ? current_user.fullname.slice(0, current_user.fullname.indexOf(' '))
              : current_user.fullname
          }}
        </v-btn>
        <v-btn text v-bind:to="{ name: 'Register' }" v-if="!current_user"
          id="register_btn">
          register
        </v-btn>
        <v-btn text v-bind:to="{ name: 'Login' }" v-if="!current_user"
          id="login_btn">
          Login
        </v-btn>
        <v-btn text @click="logout" v-if="current_user"
          id="logout_btn">
          Logout
        </v-btn>
      </v-toolbar-items>
     </v-app-bar>

    <v-main>
      <v-container fluid>
        <div id="app">
          <router-view/>
        </div>
      </v-container>
    </v-main>
    <v-footer class="text-center" color="indigo" app>
      <span class="white--text">&copy; 2021</span>
    </v-footer>
  </v-app>
</template>
<script>
import './assets/stylesheets/main.css';
import bus from './bus'
import axios from 'axios'

export default {
  name: 'App',
  data: ()=>({
    drawer: null,
    current_user: null,
  }),
  props: {
    source: String,
  },
  mounted() {
    this.fetchUser()
    this.listenToEvents()
  },
  methods: {
    listenToEvents() {
      bus.$on('refreshUser', ()=>{
        this.fetchUser()
      })
    },
    async fetchUser() {
      return axios({
        method: 'get',
        url: '/api/current_user',
      }).then((response)=>{
        console.log('current_user', response.data.current_user)
        this.current_user = response.data.current_user
      }).catch(()=>{})
    },
    logout() {
      return axios({
        method: 'get',
        url: '/api/logout',
      }).then(()=>{
        bus.$emit('refreshUser')
        this.$router.push({ name: 'Home' })
      }).catch(()=>{})
    },
  },
}
</script>
