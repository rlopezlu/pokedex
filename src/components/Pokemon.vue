<template>
  <div class="pokemon">
    <div class="">
      Search For Pokemon
    </div>
    <!-- <v-layout> -->
      <div v-for="(poke, index) in pokes">
        <single-poke :pokeName='capitalize(poke.name)' :number="index+1+offset" v-on:click.native="update(poke.name)"></single-poke>
      </div>
      <v-btn v-on:click.native="randomPokemon">I am feeling lucky! </v-btn>
        <v-btn v-on:click.native="prev">Prev</v-btn>
        {{page}}
        <v-btn v-on:click.native="next">Next</v-btn>
        <v-btn disabled v-if="selected==null">Click a pokemon above</v-btn>
        <v-btn v-else v-on:click.native="which">Search for {{this.selected}}</v-btn>
      <v-text-field
        v-on:keyup.enter.native="search"
        v-model="message"
        name="poke-name"
        label="Search by name or number"
        value="mewtwo"
        class="input-group--focused">
      </v-text-field>
      <v-btn v-on:click.native="search">Search </v-btn>
      <div v-if="error_message != null">{{error_message}}</div>
      <v-progress-circular v-if="searching==true" indeterminate class="primary--text"></v-progress-circular>
      <selected v-if="selectedData != null"
        :height="selectedData.height"
        :weight="selectedData.weight"
        :name="capitalize(selectedData.name)"
        :types="selectedData.types"
        :searching="searching"
        :image="selectedData.sprites.front_default"
        :shiny="selectedData.sprites.front_shiny">
      </selected>
    <!-- </v-layout> -->
  </div>
</template>

<script>
import SinglePoke from './ListItem.vue'
import Selected from './Selected.vue'

export default {
  name: 'Pokemon',
  components: {
    'single-poke': SinglePoke,
    'selected': Selected,
  },
  created () {
    console.log('poke root component loaded')
    var self = this
    $.get('http://pokeapi.co/api/v2/pokemon/', function (data) {
      console.log(data.results)
      self.pokes = data.results
    })
  },
  data () {
    return {
      selected: null,
      message: "",
      selectedData: null,
      pokes: null,
      error_message: null,
      searchByName: true,
      searchByNameText: "name",
      hasNext: true,
      offset: 0,
      searching: false,
      page: 0
    }
  },
  methods: {
    capitalize(string){
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    which(){
      var self = this
      this.searching = true
      console.log("pressed")
      console.log(this.selected)
      if(this.selected === null)
        return;
      console.log("fetching poke data")
      // $.get('http://pokeapi.co/api/v2/pokemon/bulbasaur/', function (data) {
      $.get('http://pokeapi.co/api/v2/pokemon/'+this.selected, function (data) {
        console.log("success")
        self.selectedData = data
        console.log(self.selectedData.name)
        console.log(self.selectedData.weight)
        self.searching =false
      })
    },
    search(){
      this.searching = true
      var self = this
      if (isNaN(this.message)){
        var name = this.message.toLowerCase()
        this.error_message = null
      }else {
        if (this.message > 721 || this.message < 1){
          //raise error
          this.error_message = "Pokemon doesn't exist"
          this.searching = false
          return
        }else{
          var name = this.message
          this.error_message = null
      }}

      // console.log(this.message)
      $.get('http://pokeapi.co/api/v2/pokemon/'+name, function (data) {
        console.log("success")
        console.log(data)
        self.selectedData = data
        self.searching =false
      })
    },
    randomPokemon(){
      this.searching = true
      var self = this
      var num = Math.floor(Math.random() * 721) + 1
      $.get('http://pokeapi.co/api/v2/pokemon/'+num, function (data) {
        console.log("success")
        console.log(data)
        self.selectedData = data
        self.searching =false
      })
    },
    next(){
      if(!this.hasNext)
        return
      this.offset+=20
      this.page++
      this.selected = null
      var self = this
      $.get('http://pokeapi.co/api/v2/pokemon/?offset='+self.offset, function (data) {
        self.hasNext = data.next
        console.log("success")
        self.pokes = data.results
      })
    },
    prev(){
      if (this.page == 0)
        return
      this.offset-=20
      this.page--
      this.hasNext = true
      this.selected = null
      var self = this
      $.get('http://pokeapi.co/api/v2/pokemon/?offset='+self.offset, function (data) {
        console.log("success")
        self.pokes = data.results
      })
    },
    update(name){
      console.log(name)
      this.selected = name
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
