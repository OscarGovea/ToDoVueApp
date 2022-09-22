<template>
  <v-container fluid class="mt-5 myContainer">
    <v-card class="myCard">
      <v-row style="height:100%;width: 105%;">
        <v-col class="containerMenuAside" sm="2">
          <menu-aside :notesLength="notesLength"/>
        </v-col>
        <v-col sm="9">
          <v-container>
            <v-row>
              <v-col>
                <InputSearchNote @onSearchNote="searchNote"/>
              </v-col>
            </v-row>
            <v-row class="containerNotes">
              <v-col sm="4" v-for="(item, index) in allNotes" v-bind:key="index" v-show="item.show">
                <CardNote :indexNote="index"
                          :noteObject="item"
                          @onDeleteNote="deleteNote"
                          @onEditNote="editNote"/>
              </v-col>
            </v-row>
          </v-container>
          <FormNewNote  :editingNote="editingNote"
                        :modeEditing="modeEditing"
                        @onSaveNote="saveNota"
                        @onCloseModal="closeModal"
                        @onUpdateNote="updateNote"/>
        </v-col>
      </v-row>
     
    </v-card>
  </v-container>
</template>

<script>
import FormNewNote from "../components/Notes/FormNewNote.vue"
import InputSearchNote from "../components/Notes/InputSearchNote.vue"
import CardNote from "../components/Notes/CardNote.vue"
import MenuAside from "../components/MenuAside.vue";
export default {
  name: "Home",
  components: {
    MenuAside,
    FormNewNote,
    InputSearchNote,
    CardNote
  },
  data: () => ({
    editingNote:{},
    modeEditing:false,
    allNotes:[
    {
      idNote:1,
      noteTitle: "Vue JS",
      noteDescription: "Start developing with VueJS 3.",
      priorityLevel:'Medium',
      itsDone: true,
      date: "13/09/2022",
      status: true,
      show: true
    },
    {
      idNote:2,
      noteTitle: "Nuxt.js",
      noteDescription: "Read Nuxt.js documentation.",
      priorityLevel:'Low',
      itsDone: true,
      date: "17/09/2022",
      status: true,
      show: true
    },
    {
      idNote:3,
      noteTitle: "Github",
      noteDescription: "Upload this ToDo app to Github pages.",
      priorityLevel:'High',
      itsDone: true,
      date: "20/09/2022",
      status: true,
      show: true
    },{
      idNote:4,
      noteTitle: "New project",
      noteDescription: "Create a new NuxtJs project.",
      priorityLevel:'High',
      itsDone: false,
      date: "22/09/2022",
      status: true,
      show: true
    },],
  }),
  methods:{
    saveNota(newNota){
      this.allNotes.push(newNota);
    },
    deleteNote(index){
      if (confirm("Do you really want to delete this note?") == true) {
        this.allNotes[index].show = false;
        this.allNotes[index].status = false;
      }
      //this.allNotes.slice(index,1);
    },
    editNote(noteObject){
      this.modeEditing = true;
      this.editingNote  = noteObject;
    },
    updateNote(noteToUpdate){
      this.editingNote = noteToUpdate;
      this.modeEditing = false;
    },
    closeModal(val){
      this.modeEditing = val;
      this.editingNote = {};
    },
    searchNote(txtSearch){
      for(let i=0; i<this.allNotes.length;i++){
        if(this.allNotes[i].noteTitle.toLowerCase().includes(txtSearch.toLowerCase()) ||
           this.allNotes[i].noteDescription.toLowerCase().includes(txtSearch.toLowerCase()) ||
           this.allNotes[i].priorityLevel.toLowerCase().includes(txtSearch.toLowerCase()) ||
           this.allNotes[i].date.toLowerCase().includes(txtSearch.toLowerCase())){
          this.allNotes[i].show = true
        }
        else{
          this.allNotes[i].show = false
        }
      }
    }
  },
  computed: {
    notesLength() {
      let notesNotDone = this.allNotes.filter(note => !note.itsDone)
      return notesNotDone.length;
    }
  }
};
</script>

<style>
  .v-application--wrap{
    background: url("../assets/fondoBG.jpg");
  }
  .myContainer, .myCard{
    height: 83vh !important;
    width: 95vw;
  }
  .myCard{
    /* background-color: #F8F9FD; */
    background: rgba(0,0,0,0.5) !important;
  }
  .containerMenuAside{
    padding-top: 0;
    padding-bottom: 0;
  }
  .v-btn--absolute.v-btn--bottom, .v-btn--fixed.v-btn--bottom{
    bottom: 70px;
  }
  .v-btn--absolute.v-btn--right, .v-btn--fixed.v-btn--right {
    right: 65px;
  }
  .containerNotes{
    overflow-x: hidden;
    overflow-y: auto;
    height: 63vh;
    margin-right: -60px;
  }
  ::-webkit-scrollbar {
      width: 5px;
}
::-webkit-scrollbar-track {
      background-color: #ef6e2887;
      border-radius: 5px;
} 
::-webkit-scrollbar-thumb {
      background-color: rgba(0, 0, 0, 0.2);
} 
</style>