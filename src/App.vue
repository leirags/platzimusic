<template lang="pug">
  #app
    img(src='https://leirags.github.io/platzimusic/dist/logo.png')
    h1 {{ title }}
    spinner(v-show="loading")
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")

</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      title: 'Platzi Music - LeirAGS',
      artists: [],
      countries: [
        { name: 'México', value: 'mexico' },
        { name: 'Argentina', value: 'argentina' },
        { name: 'Colombia', value: 'colombia' },
        { name: 'España', value: 'spain' }
      ],
      selectedCountry: 'mexico',
      loading: true
    }
  },
  components: {
    Artist: Artist,
    Spinner
  },
  methods: {
    refreshArtists() {
      const self = this
      this.loading = true
      this.artists = []
      getArtists( this.selectedCountry )
      .then( function(artists){
          self.artists = artists
          self.loading = false
        })
    }
  },
  mounted () {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
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
