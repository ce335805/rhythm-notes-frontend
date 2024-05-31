<script setup>
import {computed, onMounted, ref} from "vue";
import AddDiaryEntry from "@/components/AddDiaryEntry.vue";
import axios from "axios";

const diaryEntries = ref([])

const transformedEntries = computed(() => {
  return diaryEntries.value.map(item => {
    return {
      ...item,
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
</script>

<template>
  <div class="diaryList">
    <AddDiaryEntry @new-diary-entry-added="handleNewDiaryEntry"/>
    <ul>
      <li v-for="entry in transformedEntries" :key="entry.timeCreated" class="diaryListItem">
        <div class="tileTitle">
          <span v-html="entry.mood" class="emojiHeading"></span>
          <h2 class="listHeading">{{ entry.title }}</h2>
        </div>
        <span class="listDate">{{ entry.timeCreated }}</span>
        <p class="listContent">{{ entry.content }}</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>

.diaryList {
  width: 30%;
  margin: 0 auto;
}

.diaryListItem {
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

.tileTitle {
  display: flex;
}

.emojiHeading {
  font-size: 30px;
}

.listHeading {
  margin: 0;
}

.listContent {
  width: 100%;
  margin: 0;
}

.listDate {
}


ul {
  margin: 0;
  padding: 0;
}

</style>
