<template>
    <div class="spell">
        <h3 class="spell__name">{{ spell.name }}</h3>

        <div class="spell__school"><em>
            {{ ordinalSuffix }}, {{ spell.school.name }} <span v-if="spell.ritual">(ritual)</span>
        </em></div>

        <ul class="spell__meta">
            <li v-if="spell.casting_time"><strong>Casting Time:</strong> {{ spell.casting_time }}</li>
            <li v-if="spell.range"><strong>Range:</strong> {{ spell.range }}</li>
            <li v-if="spell.components">
                <strong>Components:</strong>
                <span :key="index" v-for="(spellComponent, index) in spell.components">
                {{ spellComponent }}
                </span>
                <span v-if="spell.material">( {{ spell.material }} )</span>
            </li>
            <li v-if="spell.duration"><strong>Duration:</strong> <span v-if="spell.concentration">Concentration,</span> {{ spell.duration }}</li>
        </ul>

        <div class="spell__desc">
            <p :key="index" v-for="(desc, index) in spell.desc" v-text="desc" />
        </div>

        <div v-if="spell.higher_level" class="spell__desc">
            <p><strong><em>At Higher Levels. </em></strong>{{ spell.higher_level[0] }}</p>
        </div>

        <p>classes: <span :key="index" v-for="(spellClass, index) in spell.classes" v-text="spellClass.name + ' '" /></p>
        <p v-if="spell.subclasses.length">subclasses: <span :key="index" v-for="(subclass, index) in spell.subclasses" v-text="subclass.name + ' '" /></p>

        <p><em>{{ spell.page }}</em></p>
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