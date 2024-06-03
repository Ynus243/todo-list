<template>
    <header class="header">
        <Transition name="header__notes" mode="out-in">
            <div class="header__notes" v-if="header">
                <button @click="changeLang">{{lang == 'ru' ? 'RU' : 'UZ'}}</button>
                <h1>{{ words && words.appbartitle && words.appbartitle[lang] }}</h1>
                <button @click="header = false">
                    <img src="@/assets/img/search.svg" alt="">
                </button>
            </div>
            <form class="header__form" v-else>
                <a href="#" class="back" @click.prevent="header = true, search = ''"><img src="@/assets/img/back.svg" alt=""></a>
                <input type="text" :placeholder="words.appbarsearch[lang]" required name="search" class="header__input" v-model="search">
                <a href="#" class="close" @click.prevent="search = ''"><img src="@/assets/img/close.svg" alt=""></a>
            </form>
        </Transition>
    </header>
</template>
<script>
export default {
    data() {
        return {
            search: '',
            header: true
        }
    },
    watch: {
        search(val){
            this.$emit('find', val)
        }
    },
    props: ['lang'],
    methods: {
        changeLang(){
            this.$emit('changeLang', this.lang == 'ru' ? 'uz' : 'ru')
        }
    },
    inject: ['words']
}
</script>
<style lang="scss">
.header__notes-enter-active,
.header__notes-leave-active {
  transition: 0.3s linear;
}

.header__notes-enter-from,
.header__notes-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>