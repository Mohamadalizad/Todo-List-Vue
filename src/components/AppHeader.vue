<template>
  <header class="d-flex justify-content-between text-white fs-4">
    <span>لیست وظایف</span>
    <span>
      <button @click="changeTheme">
        <img :src="Sun" alt="sun" v-if="defaultTheme == 'dark'" />
        <img :src="Moon" alt="moon" v-else />
      </button>
    </span>
  </header>
</template>

<script setup >
// give Sun and Moon svg
import Sun from "../assets/sun.svg";
import Moon from "../assets/moon.svg";

import { ref } from "vue";

// defaultTheme is dark in first time
const defaultTheme = ref("dark");

// toggle in function --> dark / light
function changeTheme() {
  if (defaultTheme.value == "dark") {
    defaultTheme.value = "light";
    document.querySelector("body").classList.add("light");
  } else {
    defaultTheme.value = "dark";
    document.querySelector("body").classList.remove("light");
  }
}
</script>

<style scoped>
button {
  width: 4rem;
  height: 4rem;
  color: white;
  background-color: white;
  cursor: pointer;
  border-radius: 100%;
}
img {
  width: 2rem;
  height: 2rem;
}
</style>