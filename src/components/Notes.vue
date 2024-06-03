<template>
    <div class="notes">
        <div class="container">
            <div class="notes__nav">
                <h3>{{ words && words.infobar && words.infobar[lang] }}</h3>
                <button @click="grid = !grid">
                    <img src="@/assets/img/list.svg" alt="" v-if="grid">
                    <img src="@/assets/img/grid.svg" alt="" v-else>
                    <span>{{ grid ? (words && words.list && words.list[lang]) : (words && words.grid && words.grid[lang]) }}</span>
                </button>
            </div>
            <transition-group tag="div" class="notes__grid" :class="{list: !grid}" name="notes" mode="out-in">
                <OneNote
                    v-for="note in notes"
                    :key="note.id" :note="note"
                    @remove="$emit('remove', note.id)"
                    @edit="$emit('edit', note)"
                    :lang="lang"
                 />
            </transition-group>
        </div>
    </div>
</template>
<script>
    import OneNote from './OneNote.vue'
    export default {
        components: {
            OneNote
        },
        data() {
            return {
                grid: true
            }
        },
        props: ['notes', 'lang'],
        inject: ['words']

    }
</script>
<style lang="scss">

</style>