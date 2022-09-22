<template>
    <v-card elevation="3" class="myCardNote" dark>
        <v-card-title v-on:click="editNote" style="position: relative;">
            {{noteObject.noteTitle}}
            <span class="spanDone">
                <v-tooltip top>
                    <template v-slot:activator="{ on, attrs }">
                        <v-btn :class="evaluateDone" fab x-small dark v-bind="attrs" v-on="on"> 
                            <v-icon v-if="noteObject.itsDone">mdi-check</v-icon>
                            <v-icon v-if="!noteObject.itsDone">mdi-close</v-icon>
                        </v-btn>
                    </template>
                    <span>Done?</span>
                </v-tooltip>
            </span>
        </v-card-title>
        <v-card-subtitle v-on:click="editNote">{{noteObject.date}}</v-card-subtitle>
        <v-card-text v-on:click="editNote" class="myCardText">
            <p class="textOverfloeEllipsis">{{noteObject.noteDescription}}</p>
        </v-card-text>
        <v-card-actions class="myCardActions">
            <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                    <v-chip class="ma-2" v-bind:class="evaluatePriority" v-bind="attrs" v-on="on">
                        {{noteObject.priorityLevel}}
                    </v-chip>
                </template>
                <span>Priority</span>
            </v-tooltip>
            <v-spacer></v-spacer>
            <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                    <v-btn v-on:click="editNote" class="mr-4" small icon v-bind="attrs" v-on="on">
                        <v-icon color="white lighten-1">
                            mdi-eye
                        </v-icon>
                    </v-btn>
                </template>
                <span>Details</span>
            </v-tooltip>
            <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                    <v-btn v-on:click="deleteNote" small icon v-bind="attrs" v-on="on" color="red">
                        <v-icon color="white lighten-1">
                            mdi-delete
                        </v-icon>
                    </v-btn>
                </template>
                <span>Delete</span>
            </v-tooltip>
        </v-card-actions>
    </v-card>
</template>
  
<script>
export default {
    name: "CardNote",
    props: {
        indexNote: Number,
        noteObject: Object
    },
    data: () => ({
        show: '',
    }),
    methods: {
        deleteNote(){
            this.$emit('onDeleteNote', this.indexNote)
        },
        editNote(){
            this.$emit('onEditNote', this.noteObject)
        }
    },
    computed: {
        evaluatePriority() {
            if (this.noteObject.priorityLevel == "Low")
                return "blueChip";
            if (this.noteObject.priorityLevel == "Medium")
                return "orangeChip";
            else (this.noteObject.priorityLevel == "High")
            return "redChip";
        },
        evaluateDone(){
            if (this.noteObject.itsDone)
                return "greenChip"
            else
                return "redChip"
        }
    }
};
</script>
  
<style>
.myCardNote:hover {
    background: rgba(42, 54, 64, 0.6) !important;
    border-radius: 16px !important;
    box-shadow: 0 4px 30px rgb(0 0 0 / 10%) !important;
    backdrop-filter: blur(3.3px) !important;
    -webkit-backdrop-filter: blur(3.3px);
    border: 1px solid #ffffff !important;
    cursor: pointer;
}

.myCardNote {
    max-height: 250px;
    background: rgba(0, 0, 0, 0.48) !important;
    border-radius: 16px !important;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1) !important;
    backdrop-filter: blur(3.3px) !important;
    -webkit-backdrop-filter: blur(3.3px) !important;
    border: 1px solid rgba(253, 253, 253, 0.219) !important;
    cursor: pointer;
}

.myCardText {
    max-height: 90px;
    overflow: hidden;
}

.textOverfloeEllipsis {
    text-overflow: ellipsis;
    text-align: justify;
}

.myCardActions {
    display: flex;
    justify-content: flex-end;
    margin: 10px 5px;
}

.blueChip {
    background-color: #3882c9 !important;
}

.redChip {
    background-color: #db1515 !important;
}

.orangeChip {
    background-color: orange !important;
}
.greenChip{
    background-color: green !important;
}
.spanDone {
    position: absolute;
    top: -10px;
    right: -10px;
}
</style>