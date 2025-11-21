<template>
  <navbar></navbar>

  <search-bar v-model="search_val" :getUser="getUser"></search-bar>

  <div class="bg-base-200 w-full min-h-56 p-12">
    <div v-if="currentUser" class="max-w-4xl mx-auto">
      <!-- User Header -->
      <div class="flex items-center gap-6 mb-8">
        <img
          :src="currentUser.avatar_url"
          :alt="currentUser.name"
          class="w-32 h-32 rounded-full border-4 border-base-300"
        />
        <div>
          <h1 class="text-3xl font-bold">{{ currentUser.name || currentUser.login }}</h1>
          <p class="text-lg text-base-content/70">@{{ currentUser.login }}</p>
          <p v-if="currentUser.bio" class="mt-2 text-base-content/80">
            {{ currentUser.bio }}
          </p>
        </div>
      </div>

      <!-- User Stats Grid -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
        <div class="bg-base-300 p-4 rounded-lg text-center">
          <div class="text-2xl font-bold">{{ currentUser.public_repos }}</div>
          <div class="text-sm text-base-content/70">Repositories</div>
        </div>
        <div class="bg-base-300 p-4 rounded-lg text-center">
          <div class="text-2xl font-bold">{{ currentUser.followers }}</div>
          <div class="text-sm text-base-content/70">Followers</div>
        </div>
        <div class="bg-base-300 p-4 rounded-lg text-center">
          <div class="text-2xl font-bold">{{ currentUser.following }}</div>
          <div class="text-sm text-base-content/70">Following</div>
        </div>
        <div class="bg-base-300 p-4 rounded-lg text-center">
          <div class="text-2xl font-bold">{{ currentUser.public_gists }}</div>
          <div class="text-sm text-base-content/70">Gists</div>
        </div>
      </div>

      <!-- User Details -->
      <div class="space-y-3">
        <div v-if="currentUser.email" class="flex items-center gap-2">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
            />
          </svg>
          <span>{{ currentUser.email }}</span>
        </div>

        <div v-if="currentUser.location" class="flex items-center gap-2">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
            />
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
            />
          </svg>
          <span>{{ currentUser.location }}</span>
        </div>

        <div v-if="currentUser.company" class="flex items-center gap-2">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"
            />
          </svg>
          <span>{{ currentUser.company }}</span>
        </div>

        <div v-if="currentUser.blog" class="flex items-center gap-2">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1"
            />
          </svg>
          <a :href="currentUser.blog" target="_blank" class="link link-primary">{{
            currentUser.blog
          }}</a>
        </div>

        <div v-if="currentUser.twitter_username" class="flex items-center gap-2">
          <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
            <path
              d="M23 3a10.9 10.9 0 01-3.14 1.53 4.48 4.48 0 00-7.86 3v1A10.66 10.66 0 013 4s-4 9 5 13a11.64 11.64 0 01-7 2c9 5 20 0 20-11.5a4.5 4.5 0 00-.08-.83A7.72 7.72 0 0023 3z"
            />
          </svg>
          <a
            :href="`https://twitter.com/${currentUser.twitter_username}`"
            target="_blank"
            class="link link-primary"
          >
            @{{ currentUser.twitter_username }}
          </a>
        </div>

        <div class="flex items-center gap-2 text-sm text-base-content/60">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
            />
          </svg>
          <span>Joined {{ formatDate(currentUser.created_at) }}</span>
        </div>
      </div>
    </div>
  </div>

  <render-markup :rendered="rendered"></render-markup>
</template>

<script>
import { Octokit } from "@octokit/rest";
import { marked } from "marked";
import Navbar from "@/components/Navbar.vue";
import RenderMarkup from "@/components/RenderMarkup.vue";
import SearchBar from "@/components/SearchBar.vue";

export default {
  name: "App",
  components: {
    Navbar,
    RenderMarkup,
    SearchBar,
  },
  data() {
    return {
      octokit: new Octokit({
        auth: import.meta.env.VITE_APP_TOKEN,
      }),
      auth_user: null,
      currentUser: null,
      search_val: "",
      readme: "",
    };
  },

  methods: {
    async getMe() {
      try {
        const response = await this.octokit.request("GET /user");

        if (response.status == 200) {
          this.auth_user = response.data;
          this.currentUser = response.data;

          console.log(response);
          this.getReadme(response.data.login);
        }
      } catch (error) {
        console.error(error);
      }
    },

    async getUser() {
      try {
        const response = await this.octokit.request("GET /users/{username}", {
          username: this.search_val,
        });

        if (response.status == 200) {
          this.currentUser = response.data;

          console.log(response);
          this.getReadme(response.data.login);
        }
      } catch (error) {
        console.error(error);
      }
    },

    async getReadme(user) {
      try {
        const response = await this.octokit.request("GET /repos/{owner}/{repo}/readme", {
          owner: user,
          repo: user,
          headers: { Accept: "application/vnd.github.v3+json" },
        });

        if (response.status == 200) {
          this.readme = atob(response.data.content);
          console.log(atob(response.data.content));
        }
      } catch (error) {
        console.error(error);
        this.readme = "";
      }
    },

    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toLocaleDateString("en-US", {
        year: "numeric",
        month: "long",
        day: "numeric",
      });
    },
  },

  mounted() {
    this.getMe();
  },

  computed: {
    rendered() {
      return this.readme ? marked(this.readme) : "";
    },
  },
};
</script>
