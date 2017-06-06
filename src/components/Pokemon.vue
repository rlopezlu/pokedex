<template>
  <div class="pokemon">
    <div v-for="(poke, index) in pokes">
      <single-poke :pokeName='poke.name' :number="index+1+offset" v-on:click.native="update(poke.name)"></single-poke>
    </div>
    <p v-if="selected==null">Click a pokemon</p>
    <v-btn v-else v-on:click.native="which">Search for {{this.selected}}??</v-btn>
    <v-btn v-on:click.native="randomPokemon">I am feeling lucky! </v-btn>
    <div>
      <v-btn v-on:click.native="prev">Prev</v-btn>
      {{page}}
      <v-btn v-on:click.native="next">Next</v-btn>
    </div>
    <v-text-field
      v-on:keyup.enter.native="search"
      v-model="message"
      name="poke-name"
      label="Search by name"
      value="mewtwo"
      class="input-group--focused">
    </v-text-field>
    <div v-if="error_message != null">{{error_message}}</div>
    <v-btn v-on:click.native="search">Search </v-btn>
    <!-- <v-btn v-on:click.native="searchBy">Search by Pokemon {{searchByNameText}} instead</v-btn> -->
    <v-progress-circular v-if="searching==true" indeterminate class="primary--text"></v-progress-circular>
    <selected v-if="selectedData != null"
      :height="selectedData.height"
      :weight="selectedData.weight"
      :name="selectedData.name"
      :image="selectedData.sprites.front_default"
      :shiny="selectedData.sprites.front_shiny">
    </selected>
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
      var self = this
      if (isNaN(this.message)){
        var name = this.message.toLowerCase()
        this.error_message = null
      }else {
        if (this.message > 721 || this.message < 1){
          //raise error
          this.error_message = "Pokemon doesn't exist"
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
      var self = this
      var num = Math.floor(Math.random() * 811) + 1
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
