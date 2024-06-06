<script setup>
import {computed, onMounted, ref} from "vue";
import AddDiaryEntry from "@/components/AddDiaryEntry.vue";
import axios from "axios";

const diaryEntries = ref([]);

const idExpanded = ref(0);

const clickedElement = ref(null);

const transformedEntries = computed(() => {
  return diaryEntries.value.map(item => {
    return {
      ...item,
      content: item.content.replace(/\\n/g, '<br>'),
      timeCreated: new Date(parseInt(item.timeCreated)).toLocaleString(),
    };
  });
});

onMounted(fetchDiaryEntries);

async function fetchDiaryEntries() {
  try {
    const response = await axios.get('http://localhost:8080/rhythmnotes/all');
    diaryEntries.value = response.data;
  } catch (error) {
    console.error('Error fetching diary entries:', error);
  }
}

function handleNewDiaryEntry() {
  fetchDiaryEntries()
}

function expandItem(entryID, event) {
  if(entryID === idExpanded.value){
    idExpanded.value = 0;
    const diaryEntry = event.currentTarget;
    diaryEntry.style.height = 100 + 'px';
  } else {
    if(clickedElement.value){
      clickedElement.value.style.height = 100 + 'px';
    }
    clickedElement.value = event.currentTarget;
    idExpanded.value = entryID;
    const diaryEntry = event.currentTarget;
    diaryEntry.style.height = diaryEntry.scrollHeight + 'px';
  }

}

</script>

<template>
  <div class="diaryList">
    <AddDiaryEntry @new-diary-entry-added="handleNewDiaryEntry"/>
    <ul>
      <li v-for="entry in transformedEntries" :key="entry.timeCreated" @click="expandItem(entry.id, $event)"
          class="diaryListItem">
        <span class="listDate">{{ entry.timeCreated }}</span>
        <div class="tileTitle">
          <span v-html="entry.mood" class="emojiHeading"></span>
          <h2 class="listHeading" :class="{ expandedHeading: entry.id === idExpanded, nonExpandedHeading: !(entry.id === idExpanded)}">{{ entry.title }}</h2>
        </div>
        <p class="listContent" v-html="entry.content"></p>
      </li>
    </ul>
  </div>
</template>

<style scoped>

.diaryList {
  width: 99%;
  margin: 0 auto;
  padding: 0;
}

.diaryListItem {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
  background-color: rgba(255, 226, 124, 0.39);
  margin: 5px 0;
  padding: 5px 5px;
  border-radius: 5px;
  height: 100px;
  min-height: 100px;
  transition: height 0.3s ease;
  width: 100%;
  overflow: hidden;
}

.tileTitle {
  display: flex;
  overflow: hidden;
}

.emojiHeading {
  font-size: 20px;
  position: relative;
  top: -3px;
}

.listHeading {
  margin: 0;
  font-size: 20px;
}

.expandedHeading {
  white-space: break-spaces;
}

.nonExpandedHeading {
  white-space: nowrap;
}

.listContent {
  width: 100%;
  margin: 0;
}

.listDate {
  width: 100%;
  text-align: right;
}


ul {
  margin: 0;
  padding: 0;
}

</style>
