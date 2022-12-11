<script setup>
const word = ["c", "h", "a", "r", "m"];
const answers = useState("answers", () => []);
const answer = useState("answer", () => ["c", "l", "e", "a", "n"]);
const correct = useState("correct", () => []);

watch(answer.value, () => {
    const exists = [];
    if (answer.value.length >= 5) {
        for (const [index, letter] in answer.value) {
            if (answer.value[index] === word[index]) {
                correct.value.push(index);
            };
        }
    }
    console.log(correct.value);
});
function add(index) {
    answer.value.push("s");
}
</script>
<style scoped>
p {
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
}
</style>
<template>
    <section>
        <nav class="relative top-0 left-0 w-screen border-b-2 border-black h-16 flex justify-center items-center">
            <p class="font-black text-3xl uppercase tracking-widest"> Wordle</p>
        </nav>
        <main class="mt-16">
            <div class="flex flex-wrap w-[400px]  m-auto gap-4 justify-center">
                <div class="w-16 h-16 flex items-center rounded-lg"
                    v-for="(box, index) in answers.length === 0 ? 5 : answers[0]">
                    <p class="w-full h-full text-4xl font-black uppercase text-center pt-2 text-white rounded-lg border-2 border-gray-600"
                        :class="correct.includes(index.toString()) ? 'bg-green-600' : 'bg-red-500'">{{
                                answer[index]
                        }}
                    </p>
                </div>
                <button @click="add(index)">add</button>
            </div>
        </main>
    </section>
</template>
