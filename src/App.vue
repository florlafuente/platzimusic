<template lang="pug">
  #app
    img(src='https://florlafuente.github.io/platzimusic/dist/assets/logo.png')
    h1 Platzi Music
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists"
      v-bind:artist="artist"
      v-bind:key="artist.mbid") 
</template>

<script>
import Artist from './components/artist.vue'
import Spinner from './components/spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists : [],
      countries: [
        { name: 'Argentina', value: 'argentina'},
        { name: 'Colombia', value: 'colombia'},
        { name: 'España', value: 'spain'}
      ],
      selectedCountry: 'argentina',
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
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.loading = false
          self.artists = artists
        })
    }
  },
  mounted () {
    this.refreshArtists()
  },
  watch: {
    selectedCountry: function () {
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
</style>
