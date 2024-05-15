<script setup>
import {ref} from "vue";

const diaryEntries = ref(
    [
      {id: 1, title: 'Entry 1', content: 'some text', mood: 'happy', timeCreated: '2024-05-12T12:16:48.642264Z'},
      {id: 2, title: 'Entry 2', content: 'some more text', mood: 'sunny', timeCreated: '2024-05-12T12:16:48.642264Z'},
      {id: 3, title: 'Entry 3', content: 'other text', mood: 'yummy', timeCreated: '2024-05-12T12:16:48.642264Z'},
    ]
)
</script>

<template>
    <div>
      <ul class="diaryList">
        <li v-for="entry in diaryEntries" :key="entry.id"  class="diaryListItem">
          <h2 class = "listHeading">{{entry.title}}</h2>
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
  background-color: rgba(255, 226, 124, 0.39);
  margin: 5px;
  border-radius: 5px;
}

.listHeading{
  color: #2c3e50;
  padding: 10px 0px 0px 10px;
}

.listContent{
  color: #2c3e50;
  padding: 10px 0px 10px 10px;
}

ul li
{
  list-style-type: none;
  font-family: Helvetica;
}

</style>
