<template>
  <div id="app">
    <h1>Spell List</h1>
    <div class="spells">
      <div v-bind:key="index" v-for="(spell, index) in spells" class="spell">
        <h4 class="spell__name">{{ spell.name }}</h4>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data: function () {
      return {
        spells: null,
        loading: true,
        errored: false,
        error: null
      }
    },
    mounted () {
      axios
        .get('http://www.dnd5eapi.co/api/spells/')
        .then(response => (this.spells = response.data.results))
    }
  }
</script>

<style>
.spell {
  font-family: Arial, Helvetica, sans-serif;
}
</style>
