<template>
  <div>
    <StartButton @click="getText"/>
    <div v-if="responseText">{{ responseText }}</div>
    <form @submit.prevent>
      <label for="text">
        <input type="text" name="text" v-model="currentInput">
      </label>
    </form>
  </div>
</template>

<script setup>
import StartButton from '@/components/StartButton.vue';
import { onUpdated, ref } from 'vue';

const responseText = ref(null);
const currentInput = ref(null);

async function getText() {
  try {
    const response = await fetch('https://api.quotable.io/random');
    const text = await response.json();
    responseText.value = text.content;
  } catch (error) {
    console.error('data error', error);
  }
}

function checkWord(inputWord) {
  const wordsArray = responseText.value.split(' ');
  wordsArray.forEach((word) => {
    if (inputWord === word) {
      console.log(true);
      return true;
    }
    return false;
  });
  return true;
}

// function updateInput(input) {
//   if checkWord(input) {}
// }

onUpdated(() => {
  // console.log(currentInput.value);
  // console.log(responseText.value.split(' '));
  checkWord(currentInput.value, responseText.value.split(' '));
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}
</style>
