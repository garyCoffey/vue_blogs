<template>
  <v-app
    id="app"
  >
  <v-card class="overflow-hidden">
      <v-app-bar
        color="primary"
        shrink-on-scroll
        scroll-target="#scrolling-techniques-3"
      >
        <!-- <template v-slot:img="{ props }">
          <v-img
            v-bind="props"
            gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
          ></v-img>
        </template> -->

        <!-- <v-app-bar-nav-icon></v-app-bar-nav-icon> -->

        <v-app-bar-title center>{{title}}</v-app-bar-title>

        <v-spacer></v-spacer>

        <!-- <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-heart</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn> -->

        <template
          v-slot:extension
        >
          <v-tabs
            background-color="primary"
            align-with-title
            exact-path
          >
            <v-tabs-slider color="secondary"></v-tabs-slider>
            <v-tab
              nuxt
              link
              to="/"
            >Home
            </v-tab>
            <v-tab
              nuxt
              link
              to="/games"
            >Games
            </v-tab>
            <v-tab
              nuxt
              link
              to="/blogs"
            >Blogs
            </v-tab>
          </v-tabs>
        </template>
      </v-app-bar>
      <v-sheet
        id="scrolling-techniques-3"
        class="overflow-y-auto"
        max-height="600"
      >
        <v-container fluid>
          <v-sheet>
          <v-main>
            <nuxt />
          </v-main>
          </v-sheet>
        </v-container>
      </v-sheet>
    </v-card>
    <v-footer
      app
      color="primary"
    >
      <!-- <v-tabs
        color="primary"
        exact-path
      > -->
        <v-tab
            nuxt
            link
            to="/privacyPolicy"
          >Privacy Policy
          </v-tab>
        <div style="float:right; !important">&copy; {{ new Date().getFullYear() }}</div>
      <!-- </v-tabs> -->
    </v-footer>
  </v-app>
</template>

<script>
import { defineComponent } from "@vue/composition-api"

  export default defineComponent({
    data () {
      return {
        items: [
          {
            icon: 'mdi-apps',
            title: 'Welcome',
            to: '/'
          },
          {
            icon: 'mdi-chart-bubble',
            title: 'Blogs',
            to: '/blogs'
          },
          {
            icon: 'mdi-chart-bubble',
            title: 'Games',
            to: '/games'
          }
        ],
        title: 'Play & Code'
      }
    },
    computed:{
      theme(){
        return (this.$vuetify.theme.dark) ? 'dark' : 'light'
      }
    }
  })
</script>

<style>
  /* .tabs {
    background-color: "white";
  } */
</style>
