<template>
  <Navbar
  @searchValue="search = $event"
  :lang="lang"
  @changeLang="changeLang"
  />
  <Notes 
  :notes="filterNotes"
  @delNote="deleteNote"
  @changeNote="changeNote"
  :lang="lang"
  />
  <Modal
  @addNote="addNote"
  :currentId="currentId"
  v-show="modalOpen"
  @closeModal="closeModal"
  :edit="edit"
  :editNote="editNote"
  @editedNote="editedNote"
  />
  <AddButton @openModal="openModal"/>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import Notes from '@/components/Notes.vue';
import Modal from '@/components/Modal.vue';
import AddButton from '@/components/AddButton.vue';
import langs from "@/lang";
export default {
  components: {
    Navbar,
    Notes,
    Modal,
    AddButton
  },
  data(){
    return {
      edit: false,
      modalOpen: false,
      currentId: 1,
      notes: [],
      editNote: {},
      search: '',
      lang: 'ru',
      langwords: {}
    }
  },
  created(){
    this.getNotes()
    localStorage.lang = localStorage.lang || 'ru'
    this.lang = localStorage.lang || 'ru'
    this.langwords = langs
    localStorage.words = JSON.stringify(this.langwords)
  },
  computed: {
    filterNotes(){
      return this.search ? this.notes.filter(note => note.title.toLowerCase().includes(this.search.toLowerCase())) : this.notes
    }
  },
  provide(){
    return {
      words: localStorage.words ? JSON.parse(localStorage.words) : langs 
    }
  },
  methods: {
    openModal(){
      this.modalOpen = true
    },
    closeModal(status){
      this.modalOpen = status
      setTimeout(() => this.edit = false, 500);
    },
    addNote(item){
      this.notes.push(item)
      this.modalOpen = false
    },
    deleteNote(id){
      let index = this.notes.findIndex(note => note.id == id)
      this.notes.splice(index, 1)
    },
    getNotes(){
      const localNotes = localStorage.notes
      if (localNotes) {
        this.notes = JSON.parse(localNotes)
      } 
    },
    changeNote(id){
      this.edit = this.modalOpen = true
      const pickedNote = this.notes.find(note => note.id == id)
      this.editNote = pickedNote
    },
    editedNote(editNotes){
      this.notes.forEach(note => {
        if(note.id == editNotes.id){
          note.title = editNotes.title
          note.descr = editNotes.descr
          note.date = editNotes.date
        }
      })
    },
    changeLang(val){
      this.lang = localStorage.lang = val
    }
  },
  watch: {
    notes: {
      handler(vueNotes){
        localStorage.notes = JSON.stringify(this.notes)
      },
      deep: true
    }
  }
}
</script>

<style>

</style>