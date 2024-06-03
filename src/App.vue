<template>
  <Nav 
    @find="search = $event" :lang="lang"
    @changeLang="changeLang"
  />
  <Notes 
    :notes="filterNotes"
    @remove="removeNote"
    @edit="editNote"
    :lang="lang"
  />
  <Modal 
    v-show="showModal"
    @closeModal="cleanModal"
    @addNote="addOrChangeNote"
    :curNote="curNote"
    :lang="lang"
  />
  <a href="#" class="add__note" @click.prevent="showModal = true">
    <img src="@/assets/img/edit.svg" alt="">
  </a>
</template>

<script>
  import './assets/style/main.scss'
  import Modal from './components/Modal.vue'
  import Nav from './components/Nav.vue'
  import Notes from './components/Notes.vue'
  import { v4 as uuidv4 } from "uuid";
  import langs from '@/lang'
  export default {
    components: {
      Nav,
      Notes,
      Modal
    },
    data() {
      return {
        showModal: false,
        search: '',
        notes: [],
        curNote: {},
        lang: 'ru',
        langwords: {}
      }
    },
    created(){
      localStorage.lang = localStorage.lang || 'ru'
      this.lang = localStorage.lang || 'ru'
      this.langwords = langs
      localStorage.words = JSON.stringify(this.langwords)
    },
    provide(){
      return {
         words: localStorage.words ? JSON.parse(localStorage.words) : {}
      }
    },
    methods: {
      cleanModal(){
        this.showModal = false
        this.curNote = {}
      },
      addOrChangeNote(obj){
          const note = {...obj}
          note.date = new Date().toLocaleString()
          note.id = obj.id || uuidv4() 
          if(obj.id){
             const idx = this.notes.findIndex(c => c.id == obj.id)
             if(idx != -1) this.notes[idx] = note
          }
          else this.notes.push(note)
      },
      removeNote(id){
          const idx = this.notes.findIndex(c => c.id == id)
          this.notes.splice(idx, 1)
      },
      editNote(obj){
        this.curNote = obj
        this.showModal = true
      },
      changeLang(val){
        this.lang = localStorage.lang = val
      }
    },
    computed: {
      filterNotes(){
        if(this.search){
          let se = this.search.toLowerCase()
          const filterNotes = this.notes.filter(item => {
              const title = item.title.toLowerCase()
              if(title.includes(se)) return item
          })
          return filterNotes;
        }
        else return this.notes
      }
    }
  }
</script>

<style lang="scss">
  .add__note {
    display: block;
    width: 56px;
    height: 56px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: fixed;
    bottom: 30px;
    right: 30px;
    background: linear-gradient(0deg, rgba(103, 80, 164, 0.11), rgba(103, 80, 164, 0.11)), #FFFBFE;
    border-radius: 16px;
    box-shadow: 0px 4px 8px 3px rgba(0, 0, 0, 0.15);
    filter: drop-shadow(0px 1px 3px rgba(0, 0, 0, 0.3));
  }
</style>