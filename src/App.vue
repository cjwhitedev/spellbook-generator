<template>
  <div id="app">
    <h1>Spell List</h1>
    <div class="spells">
      <div v-bind:key="index" v-for="(spell, index) in spells" class="spell">
        <h4 class="spell__name">{{ spell.name }}</h4>
        <div class="spell__desc">
          {{ spell.desc[0] }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data: function() {
      return {
        spells: [],
        loading: true,
        errored: false,
        error: null
      }
    },
    mounted() {
      var $this = this
      axios
        .get('http://www.dnd5eapi.co/api/spells/')
        .then(function (response) {
          response.data.results.forEach( function(spell) {
            axios
              .get( 'http://www.dnd5eapi.co/api/spells/' + spell.index )
              .then(responseDet => ($this.spells.push(responseDet.data)))
          })
        })
        console.log( this.spells)
    }
  }

  // I'd love to see the app automatically add every spell to the spells known page, for classes that know all but need to prepare daily.
</script>

<style lang="sass">
@import 'styles/main.scss';
</style>

