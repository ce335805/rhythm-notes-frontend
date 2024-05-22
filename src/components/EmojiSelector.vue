<script setup>
  import {ref, onMounted} from "vue";

  const emit = defineEmits(['passEmoji']);

  const showEmojiMenu = ref(false);
  const selectedEmoji = ref( '😃');
  const emojis = ref(['😀', '😁', '😂', '🤣', '😃', '😄', '😅', '😆', '😉', '😊'])

  function toggleEmojiMenu () {
    showEmojiMenu.value = !showEmojiMenu.value;
  }

  function selectEmoji(emoji) {
    this.selectedEmoji = emoji;
    this.showEmojiMenu = false;
    emit('passEmoji', emoji)
  }

  onMounted(() => {
    emit('passEmoji', selectedEmoji.value);
  });

</script>

<template>
  <div>
    <button @click="toggleEmojiMenu" class = "emojiButton">{{selectedEmoji}}</button>
    <div v-if="showEmojiMenu" class="emoji-menu">
      <span
          v-for="emoji in emojis"
          :key="emoji"
          @click="selectEmoji(emoji)"
          class="emoji-item"
      >
        {{ emoji }}
      </span>
    </div>
  </div>
</template>

<style scoped>
.emoji-menu {
  display: flex;
  flex-wrap: wrap;
  background-color: #ffffff;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: absolute;
  z-index: 1000;
}

.emoji-item {
  font-size: 24px;
  margin: 5px 2px;
  cursor: pointer;
  background-color: #ffffff;
}

.emoji-item:hover {
  background-color: #f0f0f0;
  border-radius: 5px;
}

.emojiButton{
  background-color: #ffffff;
  margin: 5px 2px;
  border-radius: 5px;
  border: 1px solid #ccc;
  font-size: 20px;
}

</style>