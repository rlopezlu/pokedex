<template>
  <div class="selected">
    <!-- This is the selected pokemon -->
    <v-card>
      {{this.name}}
      height: {{this.height}}
      weight: {{this.weight}}
      <!-- {{this.image}} -->
      <!--  disabled until hot linking is figured out-->
      <img v-if="showImage==true" :src="image"></img>
      <img v-if="showShiny==true" :src="shiny"></img>
      <v-btn v-on:click.native="imageBtn">Toggle Sprite</v-btn>
      <v-btn v-on:click.native="shinyBtn">Toggle Shiny</v-btn>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'Selected',
  props: [
    'image',
    'shiny',
    'name',
    'height',
    'weight',
  ],
  data () {
    return {
      showImage: false,
      showShiny: false
    }
  },
  methods: {
    imageBtn: function(){
      console.log("showing sprite")
      this.showImage = !this.showImage
    },
    shinyBtn: function(){
      console.log("showing sprite")
      this.showShiny = !this.showShiny
    },
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
