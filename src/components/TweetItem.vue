<template>
    <div class="tweet_item" @click="favoriteTweet(tweet.id)">
        <div class="user_profile__tweet">
            <div class="tweet_item__user">
                @{{ username }}
            </div>
            <div class="tweet_item__content">
                {{ tweet.content }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TweetItem",
    props: {
        username: {
            type: String,
            required: true
        },
        tweet: {
            type: Object,
            required: true
        }

    },
    methods: {
        favoriteTweet(id) {
            this.$emit("favorite", id)
        }
    }
}
</script>

<style>
    .tweet_item {
        padding: 20px;
        border-radius: 5px;
        background-color: white;
        border: 1px solid #DFE3E8;
        cursor: pointer;
        transition: all 0.25s ease;
        margin: 0.5rem;
    }
    .tweet_item:hover {
        transform: scale(1.1, 1.1);
    }
    .tweet_item__user {
        font-weight: bold;
    }
</style>