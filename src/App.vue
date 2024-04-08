<script setup>
  import HomeView from "./views/HomeView"
</script>


<template>
  <header><h1>Pitch Picer</h1></header>
  <main>
    <HomeView />
  </main>
</template>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: rgb(200, 200, 200);
  margin-top: 20px;
}

h1 {
  font-family: Garamond, serif;
  font-size: 5rem;
  text-decoration: overline underline double;
  font-style: italic;
  margin-bottom: 32px;
}

body {
  background-color: rgb(30, 30, 30);
}

input, textarea{
  background-color: rgb(200, 200, 200);
}
</style>
