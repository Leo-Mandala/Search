<template>
  <div class="p-6">
    <h2 class="font-semibold text-2xl text-red-500">GitHub Repositories de: {{ username }}</h2>
    <ul v-if="repositories.length" class="mt-4">
      <li v-for="repo in repositories" :key="repo.id" class="text-blue-500 hover:underline">
        <a :href="repo.html_url" target="_blank" class="block text-sm py-1">{{ repo.name }}</a>
      </li>
    </ul>
    <div v-else class="mt-4 text-gray-500">
      Aucun repositories.
    </div>
  </div>
</template>

<script>
import { Octokit } from "@octokit/core";

export default {
  props: {
    username: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      repositories: [],
    };
  },
  async created() {
    await this.getUserRepositories();
  },
  watch: {
    username: {
      handler: 'getUserRepositories',
      immediate: true,
    },
  },
  methods: {
    async getUserRepositories() {
  const octokit = new Octokit({ auth: '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' });
  try {
    const response = await octokit.request('GET /users/{username}/repos', {
      username: this.username,
      headers: {
        accept: 'application/vnd.github+json',
      },
      type: 'owner',
      sort: 'full_name',
      direction: 'desc',
      per_page: 30,
      page: 1,
    });
    this.repositories = response.data;
  } catch (error) {
    console.error("Error occurred:", error);
    if (error.status === 401) {
      console.error("Authentication failed. Check your GitHub token.");
    }
  }
},

  },
};
</script>


