<template>
  <button
    class="theme-switcher"
    aria-label="Change theme"
    @click="switchTheme"
  ></button>
</template>

<script>
export default {
  name: "ThemeSwitcher",
  methods: {
    switchTheme() {
      const theme =
        document.body.getAttribute("data-theme") === "dark" ? "light" : "dark";
      document.body.setAttribute("data-theme", theme);
      localStorage.setItem("theme", theme);
    },
  },
};
</script>

<style scoped>
.theme-switcher {
  position: fixed;
  right: 1.5rem;
  bottom: 1.5rem;
  width: 50px;
  height: 50px;
  background-color: var(--primary-color);
  border-radius: 30px;
  cursor: pointer;
  border: none;
}
</style>
