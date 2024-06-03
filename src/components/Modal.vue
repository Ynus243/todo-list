<template>
    <Transition name="modal">
        <div class="modal" @click="clean">
            <div class="modal-content" @click.stop>
                <h3>{{ curNote.id ? words.titlewindowedit[lang] : words.titlewindow[lang]}}</h3>
                <form @submit.prevent="send">
                    <input type="text" placeholder="Title" required v-model="user.title">
                    <textarea placeholder="Content" required v-model="user.text"></textarea>
                    <div class="modal-btns">
                        <button type="button" @click="clean">{{words.closebtn[lang]}}</button>
                        <button>{{ curNote.id ? words.closebtn[lang] : words.editwindowbtn[lang]}}</button>
                    </div>
                </form>
            </div>
        </div>
    </Transition>
</template>
<script>
    export default {
        data() {
            return {
                user: {
                    title: '',
                    text: ''
                }
            }
        },
        props: ['curNote', 'lang'],
        inject: ['words'],

        methods: {
            send() {
                const note = {...this.user}
                note.id = this.curNote.id || null
                this.$emit('addNote', note)
                this.clean()
            },
            clean(){
                this.$emit('closeModal')
                for (const key in this.user) this.user[key] = ''
            }
            
        },
        watch: {
            curNote() {
                if (this.curNote.id) {
                    this.user.title = this.curNote.title
                    this.user.text = this.curNote.text
                }
            }
        }
    }
</script>
<style lang="scss">

</style>