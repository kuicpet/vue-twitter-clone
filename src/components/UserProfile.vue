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
      <form class="user_profile__new_tweet" @submit.prevent="createNewTweet" :class="{ 'exceeded': newTweetCharacterCount > 180 }">
        <label for="newTweet"><strong>New Tweet</strong>({{ newTweetCharacterCount }}/180)</label>
        <textarea id="newTweet" rows="4" v-model="newTweetContent"/>
        <div class="User_profile__tweet_type">
          <label for="tweetType"><strong>Type: </strong></label>
          <select id="tweetType" class="create_tweet__type" v-model="selectedTweetType">
            <option :value="option.value" v-for="(option, index) in tweetTypes" :key="index">
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>
          Tweet!
        </button>
      </form>
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
      newTweetContent: "",
      selectedTweetType: "instant",
      tweetTypes : [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Tweet" }
      ],
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
    },
    newTweetCharacterCount() {
      return this.newTweetContent.length;
    }
  },
  methods : {
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      console.log(`favorite tweet ${id}`)
    },
    createNewTweet() {
      if(this.newTweetContent && this.selectedTweetType !== "draft") {
          this.user.tweets.unshift({
            id: this.user.tweets.length + 1,
            content: this.newTweetContent
          })
          this.newTweetContent = "";
      }
    }
  },
  mounted() {
    this.followUser();
  }
  
}
</script>

<style lang="scss" scoped>
  .user_profile {
      display: grid;
      grid-template-columns: 1fr 3fr;
      width: 100%;
      padding: 50px 5%;
        .user_profile__user-panel {
          display: flex;
          flex-direction: column;
          margin: 0 50px;
          padding: 0.4rem 2.5rem;
          background-color: white;
          border-radius: 5px;
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
        .user_profile__new_tweet {
          display: flex;
          flex-direction: column;
          border-top: 1px solid #DFE3E8;
          padding-top: 20px;
          &.exceeded {
            color: red;
            border-color: red;

            button {
              background-color: red;
              color: white;
              border: none;
            }
          }
          .user_profile__tweet_type {
            display: flex;
            justify-content: space-around;
          }
        }
        textarea {
          border: 1px solid #DFE3E8;
          border-radius: 5px;
        }

      }
    }

</style>
