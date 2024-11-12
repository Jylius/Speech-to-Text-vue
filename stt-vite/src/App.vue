<script setup>
import { ref, onMounted } from 'vue';
const transcript = ref('')
const isRecording =ref(false)

const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition;
if (!Recognition) {
  alert("Tarayıcınız konuşma tanımayı desteklemiyor.");
  // Alternatif bir yol ekleyebilir veya uyarı verebilirsiniz
}

const sr = new Recognition()

onMounted(()=>{
  sr.continuous = true
  sr.interimResults = true

  sr.onstart = () => {
    console.log("SR started krdşm")
    isRecording.value = true
  }
  
  sr.onstop = () => {
    console.log("SR durdu krdşm")
    isRecording.value = false
  }
  sr.onresult = (evt) => {
    // console.log(evt);
    const t =Array.from(evt.results)
    .map(result => result[0])
    .map(result => result.transcript)
    .join('')

    transcript.value = t
  }
})
const ToggleMic =()=>{
  if (isRecording.value) {
    sr.stop()
  } else {
    sr.start()
  }
}


</script>

<template> 
  <div class="container">
    <button 
    :class="{'mic': true, 'is-recording': isRecording}" 
    @click="ToggleMic"
    >
      {{ isRecording ? "Stop Recording" : "Start Recording" }}
    </button>
    <div class="transcript" v-text="transcript"></div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #f2f2f2;
}

.mic {
  background-color: #007bff;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.mic:hover {
  background-color: #0056b3;
}

.mic.is-recording {
  background-color: #dc3545;
}

.transcript {
  margin-top: 20px;
  padding: 10px;
  width: 100%;
  max-width: 600px;
  min-height: 100px;
  background-color: #ffffff;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: #333;
  font-size: 16px;
  overflow-y: auto;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
</style>
