<template>
  <SkipLink />
  <main class="container" id="maincontent">
    <Posts />
  </main>
  <ThemeSwitcher />
</template>

<script>
import ThemeSwitcher from "./components/ThemeSwitcherComponent.vue";
import SkipLink from "./components/SkipLinkComponent.vue";
import Posts from "./components/PostsComponent.vue";

export default {
  name: "App",
  components: { ThemeSwitcher, SkipLink, Posts },
};
</script>

<style scoped>
.container {
  padding: 0 1.5rem;
  margin-right: auto;
  margin-left: auto;
  max-width: 960px;
}
</style>
