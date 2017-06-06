<template>
  <v-app>
    <pokemon-tag></pokemon-tag>    
  </v-app>
</template>

<script>
  import Pokemon from './components/Pokemon.vue'

  export default {
    components: {
      'pokemon-tag': Pokemon
    },
    data () {
      return {
        clipped: false,
        drawer: true,
        fixed: false,
        items: [
          { icon: 'bubble_chart', title: 'Inspire' }
        ],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Vuetify.js'
      }
    }
  }
</script>

<style lang="stylus">
  @import './stylus/main'
</style>
