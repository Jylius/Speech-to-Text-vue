<template>
    <div class="speech_container">
      <transition name="fade">
        <i
          class="fa fa-microphone fa-3x"
          :class="{ isListening: isListening }"
          @click="listen"
        >
        </i>
      </transition>
      <transition name="slide">
        <p v-if="speechToText" class="speechToText">
          {{ speechToText }}
        </p>

      </transition>
    </div>
  </template>
  
  <script>
  export default {
    props:{
      addNote : {
        type : Function,
        required : true
      },
      deleteNote : {
        type : Function,
        required : true
      },
      removeAllNotes : {
        type : Function,
        required : true
      }
    },
    data() {
      return {
        speechToText: null,
        isListening: false,
        recognition:null
      };

    },
    methods: {
      listen() {
  if (!this.isListening) {
    this.isListening = true;
    this.recognition.start();
  } else {
    this.isListening = false;
    this.recognition.stop();
  }
},
      record(event){
  this.speechToText = event.results[0][0].transcript;

  const parseRegex = /(?<id>(\d+))\s*(?=nolu).*(?<command>sil)$/giu;
  const voiceMatch = parseRegex.exec(this.speechToText);

  const allNotesRemoveRegex = /tüm notları sil/giu;
  const allNotesRemoveMatch = allNotesRemoveRegex.test(this.speechToText);

  console.log(voiceMatch);
  console.log(allNotesRemoveMatch);

  setTimeout(() => {
    if (voiceMatch && voiceMatch.groups?.id && voiceMatch.groups?.command) {
      this.deleteNote(Number(voiceMatch.groups.id));
    } else if (allNotesRemoveMatch) {
      this.removeAllNotes();
    } else {
      this.addNote(this.speechToText);
    }

    this.speechToText = null;
  }, 1000);
}

    },
    mounted() {
  const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  this.recognition = new SpeechRecognition();
  this.recognition.lang = "tr-TR";
  this.recognition.onresult = this.record;
}


   };
  </script>
  
  <style scoped>
i{
  cursor: pointer;
}
.speechToText{
  margin-top: 10px;
}
.isListening{
  color: red;
  transition: all 0.5s;
}


  </style>
  