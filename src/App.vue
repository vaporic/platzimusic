<template lang="pug">
  #app
    img(src='https://vaporic.github.io/platzimusic/src/assets/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")

</template>

<script>
  import getArtists from './api'
  import Artist from './components/Artist'
  import Spinner from "./components/Spinner";

  export default {
    name: 'app',
    data() {
      return {
        artists: [],
        countries: [
          { name: 'Argentina', value: 'argentina'},
          { name: 'Colombia', value: 'colombia'},
          { name: 'España', value: 'spain'},
          { name: 'México', value: 'mexico'},
        ],
        selectedCountry: 'mexico',
        loading: true
      }
    },
    components: {
      Spinner,
      Artist
    },
    methods: {
      refreshArtist() {
        const self = this
        this.loading = true
        getArtists(this.selectedCountry)
          .then(function (artists) {
            self.loading = false
            self.artists = artists
          })
      }
    },
    mounted() {
      this.refreshArtist()
    },
    watch: {
      selectedCountry () {
        this.refreshArtist()
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
