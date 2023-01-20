<template>
  <div
    class="w-[400px] h-96 shadow-2xl bg-white absolute right-0 left-0 mx-auto top-0 bottom-0 my-auto rounded-xl flex flex-col items-center"
  >
    <p class="mt-16 text-2xl text-center flex flex-col gap-2">
      {{ defaultMessage }} <br />
      <span class="font-semibold tracking-wider">
        {{ winMessage || loseMessage }}</span
      >
    </p>

    <VictoryIcon v-if="winMessage" />
    <p v-else class="text-7xl my-8">🫢</p>
    <button
      class="bg-yellow-400 mt-6 w-28 p-2 rounded-lg border border-gray-300"
      @click="$emit('replayClick')"
    >
      Replay
    </button>
  </div>
</template>
<script>
export default {
  props: {
    winMessage: {
      type: String,
      required: false,
    },
    loseMessage: {
      type: String,
      required: false,
    },
    defaultMessage: {
      type: String,
      default: "Congratulations!",
    },
  },
  emits: ["replayClick"],
};
</script>
