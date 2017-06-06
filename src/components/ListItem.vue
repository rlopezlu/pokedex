<template>
  <div class="poke-list-item">
    <v-card v-on:click="select">
      {{this.number}}
      {{this.pokeName}}
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'ListItem',
  props: [
    'pokeName',
    'number'
  ],
  data () {
    return {
      // thisPoke: this.pokeName,
      // msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
    select : function(index){
      console.log("a button has been clicked " + this.number)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
