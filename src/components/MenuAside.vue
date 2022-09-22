<template>
  <v-navigation-drawer permanent dark color="rgba(0, 6, 18,0.73)" class="menuAside">
    <v-list>
      <v-list-item class="px-2">
        <v-list-item-avatar size="80">
          <v-avatar size="80">
            <img src="../assets/perfil.jpg" alt="John">
          </v-avatar>
        </v-list-item-avatar>
      </v-list-item>
      <v-list-item link>
        <a href="mailto:od.govea@gmail.com">
          <v-list-item-content>
            <v-list-item-title class="text-h6 myName">
              Oscar Govea
            </v-list-item-title>
            <v-list-item-subtitle>od.govea@gmail.com</v-list-item-subtitle>
          </v-list-item-content>
        </a>
      </v-list-item>
    </v-list>
    <v-divider></v-divider>
    <v-list nav dense>
      <v-list-item link>
        <v-list-item-icon>
          <v-tooltip top>
            <template v-slot:activator="{ on, attrs }">
              <v-badge :content="notesLength" :value="notesLength" color="#ff5900" overlap bordered>
                <v-icon v-bind="attrs" v-on="on">mdi-note-outline</v-icon>
              </v-badge>
            </template>
            <span>Undone notes</span>
          </v-tooltip>
        </v-list-item-icon>
        <v-list-item-title>My notes</v-list-item-title>
      </v-list-item>
      <!-- <v-list-item link>
        <v-list-item-icon>
          <v-icon>mdi-alarm</v-icon>
        </v-list-item-icon>
        <v-list-item-title>My remainders</v-list-item-title>
      </v-list-item> -->
    </v-list>
    <template v-slot:append>
        <v-row>
          <v-col class="pa-6">
            <v-tooltip top>
              <template v-slot:activator="{ on, attrs }">
                <a href="https://github.com/OscarGovea/ToDoAppVue" target="_blank">
                  <v-btn fab x-small dark v-bind="attrs" v-on="on"> 
                      <v-icon>mdi-github</v-icon>
                  </v-btn>
                </a>
              </template>
              <span>Github repository</span>
            </v-tooltip>
          </v-col>
        </v-row>
      </template>
  </v-navigation-drawer>
</template>

<script>
export default {
  name: "MenuAside",
  props: {
    notesLength: Number
  },
  data: () => ({
    msg: "hola",
  }),
  methods: {
    openNewNote() {
      alert("ok");
    },
  },
};
</script>

<style scoped>
.menuAside {
  border-radius: 4px 0px 0px 4px;
}
.myBagde {
  background-color: #ff5900;
}
.alignCenter{
  text-align:center;
}
a{
  text-decoration: none;
}
.myName{
  color:white;
}
</style>