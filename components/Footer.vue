<template>
  <v-container fluid>
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
        <div>&copy; {{ new Date().getFullYear() }}</div>
      <!-- </v-tabs> -->
    </v-footer>
  </v-container>
</template>