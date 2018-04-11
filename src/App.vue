<template lang="pug">
  #app
    img(src='https://rodrigo-gonzalez.github.io/cursovue/dist/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:artist="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bin:key="artist.mbid") 
</template>

<script>
import Spinner from './components/Spinner.vue'
import Artist from './components/Artist.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      selectedCountry: 'Argentina',
      loading: true,
      countries: [ 
        {name: 'Argentina', value: 'Argentina'},
        {name: 'Colombia', value: 'Colombia'},
        {name: 'España', value: 'Spain'}
      ]
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists: function() {
      const self = this
      this.loading = true
      getArtists(this.selectedCountry)
        .then(function(artists) {
          self.loading = false
          self.artists = artists
        })
    }
  },
  mounted: function() {
    this.refreshArtists()
  },
  watch: {
    selectedCountry: function() {
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
