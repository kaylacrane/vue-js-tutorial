<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">
        <!-- can also do v-else and v-else-if="" -->
        Admin
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
      <!-- @submit.prevent="" adds preventDefault() plus gives a function to run in its place (found in methods) -->
      <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
        <label for="newTwoot"><strong>New Twoot</strong></label>
        <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>
        <div class="user-profile__create-twoot-type">
          <label for="newTwootType"><strong>Type: </strong></label>
          <!-- v-model links the select to the selectedTwootType value in data() -->
          <select id="newTwootType" v-model="selectedTwootType">
            <!-- colon tells us that we are referencing variables/js instead of just plain string text -->
            <option
              :value="option.value"
              v-for="(option, index) in twootTypes"
              :key="index"
              >{{ option.name }}
            </option></select
          >
        </div>
        <button>Twoot</button>
      </form>
    </div>

    <!-- <CreateTwootPanel @add-twoot="addTwoot" /> -->
    <div class="user-profile__twoots-wrapper">
      <!-- v-for="" render by iterating through list (here the list of twoots) -->
      <TwootItem
        class="user-profile__twoot"
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script>
// import { reactive, computed } from "vue";
// import { useRoute } from "vue-router";
// import { users } from "../assets/users";
import TwootItem from "./TwootItem";
// import CreateTwootPanel from "../components/CreateTwootPanel";
export default {
  name: "UserProfile",
  components: { TwootItem },
  data() {
    return {
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Twoot" },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "kaylacrane",
        firstName: "Kayla",
        lastName: "Crane",
        email: "kayla.crane@gmail.com",
        isAdmin: true,
        twoots: [
          { id: 1, content: "Let's dance!" },
          { id: 2, content: "Pizza is awesome!" },
          { id: 3, content: "What's the meaning of life?" },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log("more followers now");
      }
    },
  },
  computed: {
    fullName() {
      return `${this.user.fullName} ${this.user.lastName}`;
    },
  },
  methods: {
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
      }
      this.newTwootContent = "";
    },
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      console.log(`Favorited tweet #${id}`);
    },
  },
  mounted() {
    this.followUser();
  },
};
//   components: { CreateTwootPanel, TwootItem },
//   setup() {
// const route = useRoute();
// const userId = computed(() => route.params.userId);
// const state = reactive({
//   followers: 0,
//   user: users[userId.value - 1] || users[0],
// });
// function addTwoot(twoot) {
//   state.user.twoots.unshift({
//     id: state.user.twoots.length + 1,
//     content: twoot,
//   });
// }
// return {
//   state,
//   addTwoot,
//   userId,
// };
//   },
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 50px;
  padding: 50px 5%;
  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;
    margin-bottom: auto;
    h1 {
      margin: 0;
    }
    .user-profile__admin-badge {
      background: rebeccapurple;
      color: white;
      border-radius: 5px;
      margin-right: auto;
      padding: 0 10px;
      font-weight: bold;
      margin-bottom: 20px;
    }
  }
  .user-profile__twoots-wrapper {
    display: grid;
    grid-gap: 10px;
  }
}
.user-profile__create-twoot {
  display: flex;
  flex-direction: column;
  padding-top: 20px;
}
</style>
