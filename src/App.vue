<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCounty")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")

</template>

<script>
import getArtists from './api'
import Spinner from './components/Spinner'
import Artist from './components/Artist'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Mexico', value: 'mexico'},
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'España', value: 'spain'},
      ],
      selectedCounty: 'mexico',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists() {
      const self = this
      this.artists = []
      this.loading = true
      getArtists(this.selectedCounty)
        .then(function (artists) {
          self.loading = false
          self.artists = artists
        })
    }
  },
  mounted() {
    this.refreshArtists()
  },
  watch: {
    selectedCounty() {
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
