<template>
  <div class="app">
    <img src="./assets/logo.jpg" alt="logo" class="logo" />
    <h1 class="h1">Запомни меня!</h1>
    <p>Запомни все зелёные клеточки</p>
    <Board></Board>
  </div>
</template>

<script>
import Board from "./components/Board.vue";
export default {
  name: "App",
  components: {
    Board,
  },
};
</script>

<style>
.app {
  text-align: center;
  margin-top: 60px;
}
.logo {
  width: 100px;
}
</style>
