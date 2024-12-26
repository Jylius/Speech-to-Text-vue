<template>
  <div>
    <appHeader></appHeader>
    <div id="appContainer">
      <speech
        :addNote="addNote"
        :deleteNote="deleteNote"
        :removeAllNotes="removeAllNotes"
      ></speech>

      <transition-group name="fade" tag="div">
        <noteItem
          v-for="note in notes"
          :key="note.id"
          :noteItem="note"
        ></noteItem>
      </transition-group>
    </div>
  </div>
</template>

<script>
import appHeader from './components/appHeader.vue';
import noteItem from './components/noteItem.vue';
import speech from './components/speech.vue';

export default {
  components: {
    appHeader,
    noteItem,
    speech,
  },

  data() {
    return {
      notes: [],
    };
  },
  methods: {
    addNote(note) {
      this.notes.push({
        id: this.notes.length + 1,
        note: note,
      });
    },
    deleteNote(id) {
      const index = this.notes.findIndex(n => n.id == id); // Notun indeksini bul
      if (index !== -1) { // Eğer indeks geçerliyse
        this.notes.splice(index, 1); // İlgili indeksteki notu sil
      }
    },
    removeAllNotes() {
      this.notes = [];
    },
  },
};
</script>

<style scoped>
/* Fade animasyonu */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}
.fade-enter, 
.fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
