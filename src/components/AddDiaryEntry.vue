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

function submit(){
  if (newDiaryEntry.value.title.trim() === '') return;
  newDiaryEntry.value.timeCreated = Date.now().toString();
  const diaryEntryCopy = structuredClone(toRaw(newDiaryEntry.value));

  axios.post('http://localhost:8080/rhythmnotes/add', diaryEntryCopy).then(
    response => {
      emit('newDiaryEntryAdded');
    }).catch(error => {
    console.log(error)
  })
  newDiaryEntry.value.title = "";
  newDiaryEntry.value.content = "";
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
    <input v-model="newDiaryEntry.content" placeholder="content" type="text" class = "contentInput" />
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
  padding: 3px 2px;
  margin: 5px 2px;
}
.contentInput{
  display: block;
  padding: 3px 2px;
  margin: 5px 2px;
  width: 90%;
}

.submitButton{
  margin: 5px 2px;
}

.addEntryTitle{
  margin: 5px;
}

.addEntryTile{
  margin: 5px;
  background-color: rgba(255, 226, 124, 0.39);
  width: 100%;
  min-height: 100px;
  border-radius: 5px;
}
</style>