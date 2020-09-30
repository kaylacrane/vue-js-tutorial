<template>
  <div id="app">
    @{{ user.username }} - {{ fullName }}
    <span><strong>Followers: </strong>{{ followers }}</span>
    <!-- @ is more or less equivalent to "v-" -->
    <button @click="followUser">Follow</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      followers: 0,
      user: {
        id: 1,
        username: "kaylajcrane",
        firstName: "Kayla",
        lastName: "Crane",
        email: "kayla.crane@gmail.com",
        isAdmin: true,
      },
    };
  },
  watch: {
    // can set up where something is updated (like follower count) when new data is received (perhaps from API)
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    },
  },
  computed: {
    // computed property helps make template look cleaner and easier to read
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
  },
  mounted() {
    // when we first load the component, followUser will run (can do API calls here!)
    this.followUser();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  text-align: center;
}
</style>
