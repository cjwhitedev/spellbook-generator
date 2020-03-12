<template>
  <div id="app" class="row">
      <div v-bind:key="index" v-for="(spell, index) in spells" class="spell col-12 col-md-6">
        <h3 class="spell__name">{{ spell.name }}</h3>
        <div class="spell__school"><em>
          <span v-if="spell.level == 0">Cantrip</span>
          <span v-else-if="spell.level == 1">1st</span>
          <span v-else-if="spell.level == 2">2nd</span>
          <span v-else-if="spell.level == 3">3rd</span>
          <span v-else>{{spell.level}}th</span>,
          {{ spell.school.name }} <span v-if="spell.ritual">(ritual)</span>
        </em></div>
        <ul class="spell__meta">
          <li v-if="spell.casting_time"><strong>Casting Time:</strong> {{ spell.casting_time }}</li>
          <li v-if="spell.range"><strong>Range:</strong> {{ spell.range }}</li>
          <li v-if="spell.components">
            <strong>Components:</strong>
            <span v-bind:key="index" v-for="(spellComponent, index) in spell.components">
              {{ spellComponent }}
            </span>
            <span v-if="spell.material">( {{ spell.material }} )</span>
          </li>
          <li v-if="spell.duration"><strong>Duration:</strong> <span v-if="spell.concentration">Concentration,</span> {{ spell.duration }}</li>
        </ul>
        <div class="spell__desc">
          <p v-bind:key="index" v-for="(desc, index) in spell.desc">{{ desc }}</p>
        </div>
        <div v-if="spell.higher_level" class="spell__desc">
          <p> <strong><em>At Higher Levels.</em></strong> {{ spell.higher_level[0] }}</p>
        </div>

        <p>classes: 
          <span v-bind:key="index" v-for="(spellClass, index) in spell.classes">
              {{ spellClass.name }}
          </span>
        </p>

        <p v-if="spell.subclasses.length">subclasses: 
          <span v-bind:key="index" v-for="(sublass, index) in spell.subclasses">
              {{ sublass.name }}
          </span>
        </p>

        <p><em>{{ spell.page }}</em></p>
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
    }
  }

  // I'd love to see the app automatically add every spell to the spells known page, for classes that know all but need to prepare daily.
</script>

<style lang="scss">
@import 'styles/main.scss';
</style>

