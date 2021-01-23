<template>
    <form class="user_profile__new_tweet" @submit.prevent="createNewTweet" :class="{ 'exceeded': newTweetCharacterCount > 180 }">
        <label for="newTweet"><strong>New Tweet</strong>({{ newTweetCharacterCount }}/180)</label>
        <textarea id="newTweet" rows="4" v-model="state.newTweetContent"/>
        <div class="User_profile__tweet_type">
          <label for="tweetType"><strong>Type: </strong></label>
          <select id="tweetType" class="create_tweet__type" v-model="state.selectedTweetType">
            <option :value="option.value" v-for="(option, index) in state.tweetTypes" :key="index">
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>
          Tweet!
        </button>
    </form>
</template>

<script>
import { reactive, computed } from "vue";

export default {
    name: "CreateTweet",
    setup(props, ctx) {
        const state = reactive({
            newTweetContent: "",
            selectedTweetType: "instant",
            tweetTypes : [
              { value: "draft", name: "Draft" },
              { value: "instant", name: "Instant Tweet" }
            ]
        })

        const newTweetCharacterCount = computed(() => state.newTweetContent.length)
        
        function createNewTweet() {
      if(state.newTweetContent && state.selectedTweetType !== "draft") {
          ctx.emit('add-tweet', state.newTweetContent);
          state.newTweetContent = "";
      }
    }
        return {
            state,
            newTweetCharacterCount,
            createNewTweet
        }
    }
}
</script>

<style lang="scss" scoped>
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

</style>