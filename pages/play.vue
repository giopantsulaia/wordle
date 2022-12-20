<script setup>
const correctWord = "charm";
const answer = useState("answer", () => "");
const words = useState("words", () => []);
const counter = useState("counter", () => 0);
const gameWon = useState("gameWon", () => false);
const keys = "qwertyuiopasdfghjklzxcvbnm";

function handleClickLetter(letter) {
  if (!gameWon.value) {
    if (answer.value.length < 5) {
      answer.value += letter;
    }
    if (answer.value.length === 5) {
      words.value.push(answer.value);
      console.log(words.value);
      answer.value = "";
      counter.value++;
    }
  }
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
  console.log(words.value[counter.value - 1]);
  if (words.value[counter.value - 1] === correctWord) {
    gameWon.value = true;
    setTimeout(() => alert("You Won!"), 50);
  }
  if (words.value.length >= 6 && !gameWon) {
    setTimeout(() => alert("You Lost!"), 100);
  }
});
function startOver() {
  window.location.reload();
}
function deleteLetter() {
  answer.value = answer.value.slice(0, -1);
}
</script>
<style scoped>
p {
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;
}
</style>
<template>
  <section>
    <nav
      class="relative top-0 left-0 w-screen border-b-2 border-black h-16 flex justify-center items-center"
    >
      <p class="font-bold text-3xl uppercase tracking-widest">Wordle</p>
    </nav>
    <main class="mt-16">
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
        <div
          class="w-16 h-16 flex items-center rounded-lg border"
          v-for="(box, index) in 5"
        >
          <p
            class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-900"
            :class="styles(1, index)"
          >
            {{
              words[1] ? words[1][index] : counter === 1 ? answer[index] : ""
            }}
          </p>
        </div>
        <div
          class="w-16 h-16 flex items-center rounded-lg border"
          v-for="(box, index) in 5"
        >
          <p
            class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-900"
            :class="styles(2, index)"
          >
            {{
              words[2] ? words[2][index] : counter === 2 ? answer[index] : ""
            }}
          </p>
        </div>
        <div
          class="w-16 h-16 flex items-center rounded-lg border"
          v-for="(box, index) in 5"
        >
          <p
            class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-900"
            :class="styles(3, index)"
          >
            {{
              words[3] ? words[3][index] : counter === 3 ? answer[index] : ""
            }}
          </p>
        </div>
        <div
          class="w-16 h-16 flex items-center rounded-lg border"
          v-for="(box, index) in 5"
        >
          <p
            class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-900"
            :class="styles(4, index)"
          >
            {{
              words[4] ? words[4][index] : counter === 4 ? answer[index] : ""
            }}
          </p>
        </div>
        <div
          class="w-16 h-16 flex items-center rounded-lg border"
          v-for="(box, index) in 5"
        >
          <p
            class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-900"
            :class="styles(5, index)"
          >
            {{
              words[5] ? words[5][index] : counter === 5 ? answer[index] : ""
            }}
          </p>
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
      <button
        class="absolute top-2 left-4 h-4 border-x-2 border-gray-600 rounded-lg p-6 items-center flex text-2xl"
        @click="startOver"
      >
        Retry
      </button>
    </main>
  </section>
</template>
