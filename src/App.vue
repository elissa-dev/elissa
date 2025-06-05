<template>
  <div id="app">
    <h1>Newton @Puzzle Game</h1>
    <PuzzlePage @puzzle-changed="($event) => (selectedPuzzleID = $event)" />
    <RecordsPage />
    <SliderPuzzle :puzzleId="selectedPuzzleID"/>
  </div>
</template>

<script>
import SliderPuzzle from "./components/SliderPuzzle.vue";
import PuzzlePage from "./components/Puzzles.vue";
import RecordsPage from "./components/Records.vue";
export default {
  name: "App",
  components: {
    SliderPuzzle,
    PuzzlePage,
    RecordsPage,
  },
  data() {
    console.log("Hello World");
    return { selectedPuzzleID: "cut-easy" };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  padding: 8px 10px;
  width: 125px;
  background: rgba(17, 46, 17, 0.84);
  color: white;
  font-weight: bold;
  cursor: pointer;
}
</style>
