<script setup>
import { correctWords } from "../words.js";
const correctWord =
  correctWords[Math.floor(Math.random() * correctWords.length)].toLowerCase();
const answer = useState("answer", () => "");
const words = useState("words", () => []);
const counter = useState("counter", () => 0);
const gameWon = useState("gameWon", () => false);
const gameLost = useState("gameLost", () => false);
const keys = "qwertyuiopasdfghjklzxcvbnm";

function handleClickLetter(letter) {
  if (!gameWon.value) {
    if (answer.value.length < 5) {
      answer.value += letter;
    }
    if (answer.value.length === 5) {
      setTimeout(() => {
        resetAnswer();
      }, 200);
    }
  }
}

function resetAnswer() {
  const isValidWord = correctWords.includes(
    answer.value.charAt(0).toUpperCase() + answer.value.slice(1)
  );
  if (isValidWord) {
    words.value.push(answer.value);
    answer.value = "";
    counter.value++;
  }
  answer.value = "";
}

function closeVictoryModal() {
  gameWon.value = false;
  gameLost.value = false;
  window.location.reload();
}

function styles(counter, index) {
  const includes =
    words.value.length > counter &&
    correctWord.includes(words.value[counter][index]);
  const correct =
    words.value.length > counter &&
    words.value[counter][index] === correctWord[index];
  const wrong =
    words.value.length > counter &&
    !correctWord.includes(words.value[counter][index]);
  return [
    correct
      ? "bg-green-600"
      : includes
      ? "bg-orange-500"
      : wrong
      ? "bg-red-500"
      : "bg-white",
    { "text-gray-700": !words.value[counter] },
  ];
}
watch(words.value, async () => {
  if (words.value[counter.value - 1] === correctWord) {
    gameWon.value = true;
  }
  if (words.value.length >= 6 && !gameWon.value) {
    gameLost.value = true;
  }
});
function startOver() {
  window.location.reload();
}
function deleteLetter() {
  answer.value = answer.value.slice(0, -1);
}
</script>
<template>
  <section>
    <nav
      class="relative top-0 left-0 w-screen border-b-2 border-black h-16 flex justify-center items-center"
    >
      <button
        class="absolute top-2 left-4 h-4 border-x border-opacity-40 border-gray-600 p-6 items-center flex text-2xl"
        @click="startOver"
        v-if="!gameWon"
      >
        Retry
      </button>
      <p
        class="font-medium text-4xl uppercase text-white mx-1 w-12 h-12 flex"
        v-for="letter in 'wordle'"
      >
        <span
          class="bg-green-600 w-full rounded-lg border-2 border-gray-700 text-center"
        >
          {{ letter }}
        </span>
      </p>
    </nav>
    <main
      class="mt-16"
      :class="{ 'blur-[2px] pointer-events-none': gameWon || gameLost }"
    >
      <div class="flex flex-wrap w-[400px] m-auto gap-4 justify-center">
        <div
          class="w-16 h-16 flex items-center rounded-lg border"
          v-for="(box, index) in 5"
        >
          <p
            class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-900"
            :class="styles(0, index)"
          >
            {{ words[0] ? words[0][index] : answer[index] }}
          </p>
        </div>
        <div v-for="row in 5" class="flex gap-4">
          <div
            class="w-16 h-16 flex items-center rounded-lg border"
            v-for="(box, index) in 5"
          >
            <p
              class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-900"
              :class="styles(row, index)"
            >
              {{
                words[row]
                  ? words[row][index]
                  : counter === row
                  ? answer[index]
                  : ""
              }}
            </p>
          </div>
        </div>
      </div>
      <div class="w-[700px] m-auto mt-28 flex flex-wrap justify-center">
        <button
          v-for="letter in keys"
          :key="letter"
          @click="handleClickLetter(letter)"
          class="m-2 border-2 border-gray-500 w-[53px] h-14 uppercase text-xl font-black rounded"
          :class="{ 'ml-6': letter === 'a' }"
        >
          {{ letter }}
        </button>
        <button
          class="w-20 m-2 h-14 border-2 border-gray-500 rounded text-xl font-black"
          @click="deleteLetter"
        >
          ⌫
        </button>
      </div>
    </main>
    <GameOver
      :onReplayClick="closeVictoryModal"
      winMessage="You Won!"
      v-if="gameWon"
    />
    <GameOver
      :onReplayClick="closeVictoryModal"
      loseMessage="You Lost!"
      defaultMessage="Game Over"
      v-if="gameLost"
    />
  </section>
</template>
<style scoped>
p {
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;
}
</style>
