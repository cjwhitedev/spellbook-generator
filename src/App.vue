<template>
    <div id="app" class="row">
        <SpellCard :key="index" v-for="(spellData, index) in spells" v-bind:spell="spellData" />
    </div>
</template>

<script>
    import SpellCard from './components/SpellCard.vue'
    import axios from 'axios'

    export default {
        name: 'App',
        data: () => ({
            spells: [],
            loading: true,
            errored: false,
            error: null
        }),
        components: {
            SpellCard
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

