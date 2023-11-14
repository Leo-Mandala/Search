<template>
  <div class="p-6">
    <input v-model="searchTerm" type="text" placeholder="Recherche un utilisateur Github" class="border border-gray-300 rounded-md p-2 mr-2" />
    <button @click="searchUsers" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Recherche</button>
    <div v-if="error" class="text-red-500 mt-4">{{ error }}</div>
    <div v-else class="mt-4">
      <div v-for="user in users" :key="user.id" @click="showUserRepos(user.login)" class="border border-gray-300 rounded-md p-4 flex items-center mb-4 cursor-pointer">
        <div class="flex-shrink-0">
          <img :src="user.avatar_url" :alt="user.login" class="w-12 h-12 rounded-full" />
        </div>
        <div class="ml-4">
          <h2 class="text-lg font-semibold">{{ user.login }}</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      searchTerm: '',
      users: [],
      error: ''
    };
  },
  methods: {
    searchUsers() {
      axios
        .get(`https://api.github.com/search/users?q=${this.searchTerm}`)
        .then(response => {
          this.users = response.data.items;
        })
        .catch(error => {
          this.error = 'An error occurred. Please try again later.';
          console.error(error);
        });
    },
    showUserRepos(username) {
      this.$emit('user-selected', username);
    }
  },
};
</script>


