<template>
  <v-app
    id="app"
  >
    <v-card>
      <NavBar />
      <v-sheet
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
    <Footer />
  </v-app>
</template>

<script>
export default {
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
  }
}
</script>

<style>
  .nuxt-content {
    padding: 10px;
    width: 300px;
    align-content: center;
  }
  .nuxt-content p {
    margin-top: 10px;
    margin-bottom: 10px;
  }
  @media only screen and (min-width: 600px) {
    .nuxt-content {
      margin: 30px;
      padding: 10px;
      margin-left: auto;
      margin-right: auto;
      width: 8em;
      width: 700px;
      align-content: center;
    }
    .nuxt-content p {
      margin-top: 10px;
      margin-bottom: 10px;
    }
  }
</style>
