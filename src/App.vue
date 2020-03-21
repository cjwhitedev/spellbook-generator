<template>
    <div id="app">
        <div class="filter">
            <button :key="index2" v-for="(casterClass, index2) in classes" v-text="casterClass" />
        </div>
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
            classes: [],
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

