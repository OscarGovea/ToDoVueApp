<template>
    <div>
        <v-dialog v-model="dialog" persistent max-width="800px">
            <template v-slot:activator="{ on, attrs }">
                <v-btn fab class="mx-2" dark color="#ff5900" fixed right bottom v-bind="attrs" v-on="on">
                    <v-icon dark> mdi-plus </v-icon>
                </v-btn>
            </template>
            <v-card dark color="#272b3b">
                <v-card-title>
                    <span class="text-h5">
                        <span v-if="modeEditing">Edit</span>
                        <span v-else>New</span>
                        Note
                    </span>
                </v-card-title>
                <v-card-text>
                    <v-container fluid class="mt-4">
                        <v-row>
                            <v-col>
                                <!--New Note-->
                                <v-form v-if="!modeEditing" ref="form" v-model="valid" lazy-validation>
                                    <v-text-field color="#ff5900" outlined dense v-model="newNota.noteTitle" :rules="nameRules" label="Title" required>
                                    </v-text-field>

                                    <v-textarea color="#ff5900" outlined dense rows="3" v-model="newNota.noteDescription" :rules="descriptionRules"
                                        label="Description" required></v-textarea>

                                    <v-select color="#ff5900" outlined dense v-model="newNota.priorityLevel" :items="priorityLevelOptions"
                                        :rules="[v => !!v || 'Priority is required']" label="Priority" required>
                                    </v-select>

                                    <v-checkbox color="#ff5900" v-model="newNota.itsDone" label="It's done?" required></v-checkbox>

                                    <v-btn :disabled="!valid" color="#ff7c00" class="mr-4" @click="save">
                                        Save
                                    </v-btn>

                                    <v-btn outlined color="#ff7c00" class="mr-4" @click="cancel">
                                        Cancel
                                    </v-btn>
                                </v-form>
                                <!--Edit note-->
                                <v-form v-else ref="form" v-model="valid" lazy-validation>
                                    <v-text-field  color="#ff5900"  outlined dense v-model="noteToUpdate.noteTitle" :rules="nameRules" label="Title" required>
                                    </v-text-field>

                                    <v-textarea color="#ff5900" outlined dense rows="3" v-model="noteToUpdate.noteDescription" :rules="descriptionRules"
                                        label="Description" required></v-textarea>

                                    <v-select color="#ff5900" outlined dense v-model="noteToUpdate.priorityLevel" :items="priorityLevelOptions"
                                        :rules="[v => !!v || 'Priority is required']" label="Priority" required>
                                    </v-select>

                                    <v-checkbox color="#ff5900" v-model="noteToUpdate.itsDone" label="It's done?" required></v-checkbox>

                                    <v-btn :disabled="!valid" color="#ff7c00" class="mr-4" @click="updateNote">
                                        Update
                                    </v-btn>

                                    <v-btn outlined color="#ff7c00" class="mr-4" @click="cancel">
                                        Cancel
                                    </v-btn>
                                </v-form>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-card-text>
            </v-card>
        </v-dialog>
    </div>
</template>
  
<script>
export default {
    name: "FormNewNote",
    props:{
        editingNote:Object,
        modeEditing: Boolean
    },
    data: () => ({
        dialog: false,
        valid: true,
        noteToUpdate:{},
        editingMode:false,
        newNota:{
            idNote: Date.now(),
            noteTitle: '',
            noteDescription: '',
            date:'',
            priorityLevel: null,
            itsDone: false,
            colorBG: "",
            status: true,
            show:true
        },

        nameRules: [
            v => !!v || 'Title is required',
        ],
        descriptionRules: [
            v => !!v || 'Description is required',
        ],
        
        priorityLevelOptions: [
            'Low',
            'Medium',
            'High',
        ],
        
        
    }),
    methods: {
        padTo2Digits(num) {
            return num.toString().padStart(2, '0');
        },
        formatDate(date) {
            return [
                this.padTo2Digits(date.getDate()),
                this.padTo2Digits(date.getMonth() + 1),
                date.getFullYear(),
            ].join('/');
        },
        updateNote(){
            this.$emit('onUpdateNote', this.noteToUpdate)
        },
        save() {
            let dateToday = this.formatDate(new Date());
            let myNewNoteToEmit = {
                idNote: Date.now(),
                noteTitle: this.newNota.noteTitle,
                noteDescription: this.newNota.noteDescription,
                date:dateToday,
                priorityLevel: this.newNota.priorityLevel,
                itsDone: this.newNota.itsDone,
                colorBG:this.newNota.colorBG,
                status: true,
                show:true
            }
            this.$emit('onSaveNote', myNewNoteToEmit)
            this.$refs.form.validate();
            this.dialog =false;
            this.resetNewNote();
        },
        cancel() {
            this.dialog = false
            this.editingMode = false;
            this.$emit('onCloseModal', this.dialog)
            this.resetNewNote();
        },
        resetValidation() {
            this.$refs.form.resetValidation()
        },
        resetNewNote(){
            this.$refs.form.resetValidation()
            this.newNota.noteTitle= '',
            this.newNota.noteDescription= '',
            this.newNota.priorityLevel= null,
            this.newNota.itsDone= false,
            this.newNota.colorBG= "",
            this.newNota.status= true,
            this.newNota.show=true
            
            this.noteToUpdate = {};
        }
    },
    watch:{
        modeEditing(){
            this.dialog = this.modeEditing;
            this.editingMode = this.modeEditing;
            //this.$emit('onCloseModal', this.dialog)
        },
        editingNote(){
            this.noteToUpdate = this.editingNote;
        }
    }
};
</script>