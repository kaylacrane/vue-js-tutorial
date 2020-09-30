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
    </div>

    <!-- <CreateTwootPanel @add-twoot="addTwoot" /> -->
    <div class="user-profile__twoots-wrapper">
      <!-- v-for="" render by iterating through list (here the list of twoots) -->
      <div
        class="user-profile__twoot"
        v-for="twoot in user.twoots"
        :key="twoot.id"
      >
        {{ twoot.content }}
      </div>
    </div>
  </div>
</template>

<script>
// import { reactive, computed } from "vue";
// import { useRoute } from "vue-router";
// import { users } from "../assets/users";
// import TwootItem from "../components/TwootItem";
// import CreateTwootPanel from "../components/CreateTwootPanel";
export default {
  name: "UserProfile",
  data() {
    return {
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
          { id: 2, content: "What's the meaning of life?" },
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
    followUser() {
      this.followers++;
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
    }
  }
  .user-profile__twoots-wrapper {
    display: grid;
    grid-gap: 10px;
    margin-bottom: auto;
  }
}
</style>
