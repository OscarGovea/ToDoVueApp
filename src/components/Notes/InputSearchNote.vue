<template>
    <div>
        <v-text-field background-color="rgba(255,255,255,0.8)" 
                      color="#ff5900" 
                      v-on:keyup="fncSearchNote"  
                      outlined dense 
                      v-model="txtSearch" 
                      append-icon="mdi-magnify" 
                      rounded 
                      placeholder="Search note by title, description, priority or date.">
        </v-text-field>
    </div>
  </template>
  
  <script>
  export default {
    name: "InputSearchNote",
    data: () => ({
        txtSearch: "",
    }),
    methods: {
        fncSearchNote() {
            this.$emit('onSearchNote', this.txtSearch)
        },
    },
  };
  </script>