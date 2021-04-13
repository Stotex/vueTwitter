<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile__follower-count">
        <strong>Followers:</strong> {{ followers }}
      </div>
      <form
        class="user-profile__create-tweet"
        @submit.prevent="createNewTweet"
        :class="{ exceeded: newTweetCharacterCount > 160 }"
      >
        <label for="newTweet"
          ><strong>New Tweet</strong>({{ newTweetCharacterCount }}/160)</label
        >
        <textarea id="newTweet" rows="4" v-model="newTweetContent" />

        <div class="user-profile__create-tweet-type">
          <label for="newTweetType"><strong>Type: </strong></label>
          <select id="newTweetType" v-model="selectedTweetType">
            <option
              :value="option.value"
              v-for="option in tweetTypes"
              :key="option.id"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>Tweet!</button>
      </form>
    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem
        v-for="tweet in user.tweets"
        :key="tweet.id"
        :username="user.username"
        :tweet="tweet"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import TweetItem from "./TweetItem";

export default {
  name: "UserProfile",
  components: { TweetItem },
  data() {
    return {
      newTweetContent: "",
      selectedTweetType: "instant",
      tweetTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Tweet" },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "Kenno905",
        firstName: "Kenno",
        lastName: "Pajunurm",
        email: "kennopajunurm@hotmail.com",
        isAdmin: true,
        tweets: [
          { id: 1, content: "Vue seems interesting!" },
          {
            id: 2,
            content: "I am looking for cool projects to develop my skills!",
          },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    },
  },
  computed: {
    fullname() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
    newTweetCharacterCount() {
      return this.newTweetContent.length;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      console.log(`Favourite Tweet #${id}`);
    },
    createNewTweet() {
      if (this.newTweetContent && this.selectedTweetType !== "draft") {
        this.user.tweets.unshift({
          id: this.user.tweets.length + 1,
          content: this.newTweetContent,
        });
        this.newTweetContent = "";
      }
    },
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 50px;
  padding: 50px 5%;
}

.user-profile__admin-badge {
  border: 1px solid #dfe3e8;
  margin: 5px auto 10px 0px;
  font-weight: bold;
  padding: 2px;
}

.user-profile__tweets-wrapper {
  display: grid;
  grid-gap: 10px;
  margin-bottom: auto;
}

.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
  margin-bottom: auto;
}

.user-profile__create-tweet {
  display: flex;
  flex-direction: column;
  padding-top: 20px;
}

h1 {
  margin: 0;
}
</style>