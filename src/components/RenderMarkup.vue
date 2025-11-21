<template>
  <main class="p-8">
    <div class="readme-container p-2 shadow-2xl bg-black/50">
      <div class="flex flex-col justify-center items-center">
        <div v-if="rendered">
          <div v-html="rendered"></div>
        </div>
        <div v-else>
          <h4>User has no ReadMe to show</h4>
        </div>
      </div>
    </div>
  </main>
</template>
<style scoped>
.readme-container {
  background-image: url("../assets/aero_glass.png");
  background-size: cover;
  background-position: center;
}
</style>

<script>
export default {
  props: {
    rendered: String,
  },
};
</script>
