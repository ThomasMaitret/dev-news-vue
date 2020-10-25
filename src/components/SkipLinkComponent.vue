<template>
  <a class="skip-link" href="#maincontent">Skip to main</a>
</template>

<script>
export default {
  name: "SkipLink",
};
</script>

<style scoped>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000000;
  color: white;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
</style>
