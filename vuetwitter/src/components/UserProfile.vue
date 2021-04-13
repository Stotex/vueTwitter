<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile__follower-count">
        <strong>Followers:</strong> {{ followers }}
      </div>
      <CreateTweetPanel @add-tweet="addTweet" />
    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem
        v-for="tweet in user.tweets"
        :key="tweet.id"
        :username="user.username"
        :tweet="tweet"
      />
    </div>
  </div>
</template>

<script>
import TweetItem from "./TweetItem";
import CreateTweetPanel from "./CreateTweetPanel";

export default {
  name: "UserProfile",
  components: { TweetItem, CreateTweetPanel },
  data() {
    return {
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
            content: "Hit me up with cool projects so i can develop my skills!",
          },
        ],
      },
    };
  },
  methods: {
    addTweet(tweet) {
      this.user.tweets.unshift({
        id: this.user.tweets.length + 1,
        content: tweet,
      });
    },
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
  background: rebeccapurple;
  color: white;
  padding: 5px;
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