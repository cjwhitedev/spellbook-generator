<template>
    <div class="spell">
        <h3 class="spell__name">{{ spell.name }}</h3>

        <div class="spell__school">
            {{ ordinalSuffix }}, {{ spell.school.name }} <span v-if="spell.ritual">(ritual)</span>
        </div>

        <ul class="spell__meta">
            <li v-if="spell.casting_time">Casting Time: {{ spell.casting_time }}</li>
            <li v-if="spell.range">Range: {{ spell.range }}</li>
            <li v-if="spell.components">
                Components:
                <span :key="index" v-for="(spellComponent, index) in spell.components" v-text="spellComponent + ' '" />
                <span v-if="spell.material" v-text="spell.material" />
            </li>
            <li v-if="spell.duration">Duration: <span v-if="spell.concentration">Concentration,</span> {{ spell.duration }}</li>
        </ul>

        <div class="spell__desc">
            <p :key="index" v-for="(desc, index) in spell.desc" v-text="desc" />
        </div>

        <div v-if="spell.higher_level" class="spell__desc">
            <p>At Higher Levels. <span :key="index" v-for="(higherLevel, index) in spell.higher_level" v-text="higherLevel" /></p>
        </div>

        <p>classes: <span :key="index" v-for="(spellClass, index) in spell.classes" v-text="spellClass.name + ' '" />
        <p v-if="spell.subclasses.length">subclasses: <span :key="index" v-for="(subclass, index) in spell.subclasses" v-text="subclass.name + ' '" />

        <p>{{ spell.page }}</p>
    </div>
</template>

<script>
    export default {
        name: 'SpellCard',
        props: {
            spell: Object
        },
        data() {
            return {};
        },
        computed: {
            ordinalSuffix() {
                if ( this.spell.level == 0 ) {
                    return 'Cantrip'
                } else if ( this.spell.level == 1 ) {
                    return this.spell.level + 'st'
                } else if ( this.spell.level == 2 ) {
                    return this.spell.level + 'nd'
                } else if ( this.spell.level == 3 ) {
                    return this.spell.level + 'rd'
                } else {
                    return this.spell.level + 'th'
                }
            }
        }
    }
</script>