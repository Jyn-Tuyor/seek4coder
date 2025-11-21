<template>
  <navbar></navbar>

  <search-bar v-model="search_val" :getUser="getUser"></search-bar>

  <div class="bg-base-200 w-full h-56 p-12">
    <div>
      <h3>{{ auth_username }}</h3>
    </div>
  </div>

  <render-markup :rendered="rendered"></render-markup>
</template>

<script>
// import { Octokit } from 'octokit';
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
      auth_avatar_url: "",
      auth_username: "",
      search_val: "",
      searched_avatar_url: "",
      searched_username: "",
      readme: "",
    };
  },

  methods: {
    async getMe() {
      try {
        const response = await this.octokit.request("GET /user");

        if (response.status == 200) {
          this.auth_avatar_url = response.data.avatar_url;
          this.auth_username = response.data.name;

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
          this.searched_avatar_url = response.data.avatar_url;
          this.searched_username = response.data.name;

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
          this.searched_avatar_url = response.data.avatar_url;
          this.searched_username = response.data.name;

          this.readme = atob(response.data.content);
          console.log(atob(response.data.content));
        }
      } catch (error) {
        console.error(error);
        this.readme = null;
      }
    },
  },

  mounted() {
    this.getMe();
  },

  computed: {
    rendered() {
      return marked(this.readme);
    },
  },
};
</script>
