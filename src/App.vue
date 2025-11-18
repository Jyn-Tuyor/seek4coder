<template>

  <div class="px-12 py-8 flex gap-8">
    <label class="input">
      <svg class="h-[1em] opacity-50" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
        <g stroke-linejoin="round" stroke-linecap="round" stroke-width="2.5" fill="none" stroke="currentColor">
          <path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"></path>
          <circle cx="12" cy="7" r="4"></circle>
        </g>
      </svg>
      <input type="text" required placeholder="Username" />
    </label>
    <button class="btn">Search</button>
  </div>

  <main class="p-8">
    <div class="readme-container p-2 shadow-2xl bg-black/50">
      <div class="flex flex-col justify-center items-center">
        <div v-html="rendered"></div>
      </div>
    </div>
  </main>

</template>

<style scoped>
.readme-container {
  background-image: url('./assets/aero_glass.png');
  background-size: cover;
  background-position: center;
}
</style>

<script>
// import { Octokit } from 'octokit';
import { Octokit } from '@octokit/rest';
import { marked } from 'marked';


export default {
  name: 'App',
  data() {
    return {
      octokit: new Octokit({
        auth: import.meta.env.VITE_APP_TOKEN
      }),
      auth_avatar_url: '',
      auth_username: '',
      search_val: '',
      searched_avatar_url: '',
      searched_username: '',
      readme: ''
    }
  },

  methods: {
    async getMe() {
      try {
        const response = await this.octokit.request('GET /user')

        if (response.status == 200) {
          this.auth_avatar_url = response.data.avatar_url;
          this.auth_username = response.data.name
        }

        console.log(response)
        this.getReadme(response.data.login)

      } catch (error) {
        console.error(error)
      }
    },

    async getUser() {
      try {
        const response = await this.octokit.request('GET /users/{username}', {
          username: this.search_val
        })

        if (response.status == 200) {
          this.searched_avatar_url = response.data.avatar_url;
          this.searched_username = response.data.name
        }

        console.log(response)

        await this.getReadme();

      } catch (error) {
        console.error(error)
      }
    },

    async getReadme(user) {
      try {
        const response = await this.octokit.request('GET /repos/{owner}/{repo}/readme', {
          owner: user,
          repo: user,
          headers: { Accept: "application/vnd.github.v3+json" }
        })

        if (response.status == 200) {
          this.searched_avatar_url = response.data.avatar_url;
          this.searched_username = response.data.name
        }


        this.readme = atob(response.data.content)
        console.log(atob(response.data.content))

      } catch (error) {
        console.error(error)
      }
    }


  },

  mounted() {
    this.getMe();
  },

  computed: {
    rendered() {
      return marked(this.readme)
    }
  }
}


</script>
