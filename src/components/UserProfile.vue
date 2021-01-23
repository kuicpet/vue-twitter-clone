<template>
  <div class="user_profile">
    <div class="user_profile__user-panel">
      <h1 class="user_profile__username">{{ user.username }}</h1>
      <div class="user_profile__main-badge" v-if="user.isAdmin">
          Admin
      </div>
      <div class="user_profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
    </div>
    <div class="user_profile__tweets-wrapper">
        <TweetItem 
          v-for="tweet in user.tweets" 
          :key="tweet.id" 
          :username="user.username" 
          :tweet="tweet"
          @favorite="toggleFavorite"
        />
    </div>
  </div>
</template>

<script>
import TweetItem from "./TweetItem";

export default {
  name: 'App',
  components: {
    TweetItem
  },
  data() {
    return {
      followers: 0,
      user : {
        id: 1,
        username: "kuicpet",
        firstName: "Kingsley",
        lastName: "Umujeyan",
        email: "kingsleyumujeyan@gmail.com",
        isAdmin: true,
        tweets: [
            { id: 1, content: "Tweets" },
            { id: 2, content: "More Tweets" }
        ]
      }
    }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if(oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower`);
      }
    }
  },
  computed : {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    }
  },
  methods : {
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      console.log(`favorite tweet ${id}`)
    }
  },
  mounted() {
    this.followUser();
  }
  
}
</script>

<style>
    .user_profile {
        display: grid;
        grid-template-columns: 1fr 3fr;
        width: 100%;
        padding: 50px 5%;
    }
    .user_profile__user-panel {
        display: flex;
        flex-direction: column;
        margin-right: 50px;
        padding: 0.4rem 2.5rem;
        background-color: white;
        border-radius: 5px;
        border: 1px solid black;
    }
    h1 {
        margin: 0;
    }
    .user_profile__username {
        margin: 0.4rem 0;
    }
    .user_profile__main-badge {
        font-size: 0.9rem;
        margin-right: auto;
        font-weight: bold;
        background-color: rebeccapurple;
        color: white;
        padding: 0.1rem 0.5rem;
        border-radius: 5px;
    }
    .user_profile__follower-count {
        margin: 0.5rem 0;
    }
</style>
