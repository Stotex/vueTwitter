<template>
  <form
    class="create-tweet-panel"
    @submit.prevent="createNewTweet"
    :class="{ exceeded: newTweetCharacterCount > 160 }"
  >
    <label for="newTweet"
      ><strong>New Tweet</strong> ({{ newTweetCharacterCount }}/160)</label
    >
    <textarea id="newTweet" rows="4" v-model="state.newTweetContent" />

    <div class="create-tweet-panel__submit">
      <div class="create-tweet-type" id="create-tweet-type-id">
        <label for="newTweetType"><strong>Type: </strong></label>
        <select id="newTweetType" v-model="state.selectedTweetType">
          <option
            :value="option.value"
            v-for="(option, index) in state.tweetTypes"
            :key="index"
          >
            {{ option.name }}
          </option>
        </select>
      </div>

      <button id="tweetButton">Tweet It!</button>
    </div>
  </form>
</template>

<script>
import { reactive, computed } from "vue";
export default {
  name: "CreateTweetPanel",
  setup(props, ctx) {
    const state = reactive({
      newTweetContent: "",
      selectedTweetType: "instant",
      tweetTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Tweet" },
      ],
    });
    const newTweetCharacterCount = computed(() => state.newTweetContent.length);
    function createNewTweet() {
      if (state.newTweetContent && state.selectedTweetType !== "draft") {
        ctx.emit("add-tweet", state.newTweetContent);
        state.newTweetContent = "";
      }
    }
    return {
      state,
      newTweetCharacterCount,
      createNewTweet,
    };
  },
};
</script>



<style scoped>
.create-tweet-panel {
  margin-top: 20px;
  padding: 20px 0;
  display: flex;
  flex-direction: column;
}
.create-tweet-panel__submit {
  display: flex;
  justify-content: space-between;
}
#newTweet {
  border: 1px solid #dfe3e8;
  border-radius: 5px;
}
#create-tweet-type-id {
  padding: 10px 0;
}
#tweetButton {
  padding: 5px 20px;
  margin: auto 0;
  border-radius: 5px;
  border: none;
  background-color: deeppink;
  color: white;
  font-weight: bold;
}
.exceeded {
  color: red;
  border-color: red;
}
</style>