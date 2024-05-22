<script setup>
import {computed, ref} from "vue";
import AddDiaryEntry from "@/components/AddDiaryEntry.vue";

const diaryEntries = ref([
  {id: 2, title: "Title 1", content: "This is some test content", mood: '😃', timeCreated: Date.now()},
  {id: 3, title: "Title 2", content: "This is some more test content within the second test item", mood: '🤣', timeCreated: Date.now()}
])

const transformedEntries = computed(() => {
  return diaryEntries.value.map(item => {
    return {
      ...item,
      timeCreated: new Date(item.timeCreated).toLocaleString(),
    };
  });
});

function handleNewDiaryEntry(newDiaryEntry){
  diaryEntries.value.push(newDiaryEntry);
}

</script>

<template>
    <div class="diaryList">
      <AddDiaryEntry @pass-new-diary-entry="handleNewDiaryEntry"/>
      <ul>
        <li v-for="entry in transformedEntries" :key="entry.timeCreated" class="diaryListItem">
          <div class = "tileTitle">
            <span v-html="entry.mood" class = "emojiHeading"></span>
            <h2 class = "listHeading">{{entry.title}}</h2>
          </div>
            <span class = "listDate">{{entry.timeCreated}}</span>
          <p class = "listContent">{{entry.content}}</p>
        </li>
      </ul>
    </div>
</template>

<style scoped>

.diaryList{
  width: 30%;
  margin: 0 auto;
}

.diaryListItem{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
  background-color: rgba(255, 226, 124, 0.39);
  margin: 5px;
  border-radius: 5px;
  min-height: 100px;
  max-height: 100px;
  width: 100%;
}

.tileTitle{
  display: flex;
}

.emojiHeading{
  font-size: 30px;
}

.listHeading{
  margin: 0;
}

.listContent{
  width: 100%;
  margin: 0;
}

.listDate{
}


ul {
  margin: 0;
  padding: 0;
}

</style>
