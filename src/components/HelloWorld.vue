<template>
  <div>
    <SearchUsers @user-selected="onUserSelected" />
    <UserRepositories :username="selectedUsername" />
  </div>
</template>

<script>
import SearchUsers from './SearchUser.vue';
import UserRepositories from './SearchRepositories.vue';

export default {
  components: {
    SearchUsers,
    UserRepositories
  },
  data() {
    return {
      selectedUsername: ''
    };
  },
  methods: {
    onUserSelected(username) {
      this.selectedUsername = username;
    }
  }
};
</script>


