<script setup>

import {ref, toRaw} from "vue";
import EmojiSelector from "@/components/EmojiSelector.vue";
import { v4 as uuidv4 } from 'uuid';
import axios from 'axios';

const emit = defineEmits(['newDiaryEntryAdded']);

const hideForm = ref(false);
const newDiaryEntry = ref({title: "", content: "", mood: null, timeCreated: null});

function handleEmoji(emoji){
  newDiaryEntry.value.mood = emoji;
}

function autoExpand(event){
  const textarea = event.target;
  textarea.style.height = 'auto';
  textarea.style.height = textarea.scrollHeight + 6 + 'px';
}

function submit(){
  if (newDiaryEntry.value.title.trim() === '') return;
  newDiaryEntry.value.timeCreated = Date.now().toString();
  newDiaryEntry.value.content = newDiaryEntry.value.content.replace(/\n/g, '\\n');
  const diaryEntryCopy = structuredClone(toRaw(newDiaryEntry.value));

  axios.post('http://localhost:8080/rhythmnotes/add', diaryEntryCopy).then(
    response => {
      emit('newDiaryEntryAdded');
    }).catch(error => {
    console.log(error)
  })
  newDiaryEntry.value.title = "";
  newDiaryEntry.value.content = "";
  resetTextareaHeight();
}

function resetTextareaHeight() {
  const textarea = this.$refs.textarea;
  textarea.style.height = 'auto';
  textarea.style.height = `${textarea.scrollHeight}px`;
  textarea.rows = 2;
}

</script>

<template>
<div class = "addEntryTile">
  <div v-if="!hideForm">
    <h1 class = "addEntryTitle">Add a new entry:</h1>
    <div class = "setHeading">
      <input v-model="newDiaryEntry.title" placeholder="title" type="text" class = "titleInput" />
      <EmojiSelector @pass-emoji="handleEmoji" />
    </div>
    <textarea @input="autoExpand" v-model="newDiaryEntry.content" placeholder="content" type="text" class = "contentInput" ref="textarea"/>
    <button @click="submit" class = "submitButton">Submit</button>
  </div>
  <div v-if="hideForm" class = "plusSign">
    <div class="horizontal-bar"/>
    <div class="vertical-bar"/>
  </div>
  </div>
</template>

<style scoped>

.setHeading{
  display: flex;
}

.titleInput{
  display: block;
  padding: 3px 3px;
  margin: 5px 2px;
}
.contentInput{
  display: block;
  padding: 3px 3px;
  margin: 5px 2px;
  width: 100%;
  resize: none;
}

.submitButton{
  margin: 5px 2px;
}

.addEntryTitle{
  margin: 5px;
}

.addEntryTile{
  margin: 5px 0;
  padding: 5px 5px;
  background-color: rgba(255, 226, 124, 0.39);
  width: 100%;
  min-height: 100px;
  border-radius: 5px;
}
</style>