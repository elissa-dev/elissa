<template>
  <div>
    <h1>Swap the Images to Win</h1>
    <button @click="start" id="start-button">Start Game</button>
    <button @click="stop" id="stop-button">Quit</button>
    <p>Elapsed Time: {{ elapsedTime }}</p>
    <h1 v-if="isWinning">You Have Won!!</h1>
    <div class="row">
      <div
        class="column"
        v-for="(s, index) of shuffledPuzzleArray"
        :key="s"
        @click="swap(index)"
      >
        <img :src="require(`../assets/${puzzleId}/${s}`)" />
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

const correctPuzzleArray = [
  "image_part_001.jpg",
  "image_part_002.jpg",
  "image_part_003.jpg",
  "image_part_004.jpg",
  "image_part_005.jpg",
  "image_part_006.jpg",
  "image_part_007.jpg",
  "image_part_008.jpg",
  "image_part_009.jpg",
];

export default {
  name: "SliderPuzzle",
  props: {
    puzzleId: {
      type: String,
      default: "cut-easy",
    },
  },
  data() {
    return {
      correctPuzzleArray,
      shuffledPuzzleArray: [...correctPuzzleArray].sort(
        () => Math.random() - 0.5
      ),
      indexesToSwap: [],
      timer: undefined,
      startDateTime: new Date(),
      currentDateTime: new Date(),
    };
  },
  computed: {
    isWinning() {
      return this.shuffledPuzzleArray.every(
        (val, i) => val === correctPuzzleArray[i]
      );
    },
    elapsedDiff() {
      const current = moment(this.currentDateTime);
      const start = moment(this.startDateTime);
      return current.diff(start);
    },
    elapsedTime() {
      return moment.utc(this.elapsedDiff).format("HH:mm:ss");
    },
  },
  methods: {
    swap(index) {
      if (!this.timer) return;
      if (this.indexesToSwap.length < 2) {
        this.indexesToSwap.push(index);
      }
      if (this.indexesToSwap.length === 2) {
        const [i1, i2] = this.indexesToSwap;
        [this.shuffledPuzzleArray[i1], this.shuffledPuzzleArray[i2]] = [
          this.shuffledPuzzleArray[i2],
          this.shuffledPuzzleArray[i1],
        ];
        this.indexesToSwap = [];
      }
    },
    start() {
      this.resetTime();
      this.shuffledPuzzleArray = [...correctPuzzleArray].sort(
        () => Math.random() - 0.5
      );
      this.indexesToSwap = [];
      this.timer = setInterval(() => {
        this.currentDateTime = new Date();
        if (this.isWinning) {
          this.stop();
        }
      }, 1000);
    },
    stop() {
      this.resetTime();
      clearInterval(this.timer);
      this.timer = null;
    },
    resetTime() {
      this.startDateTime = new Date();
      this.currentDateTime = new Date();
    },
  },
};
</script>

<style scoped>
.row {
  display: flex;
  flex-wrap: wrap;
  max-width: 70vw;
  margin: auto;
  margin-bottom: 60px;
  padding: 0;
  gap: 0;
}

.column {
  width: 33.3333%;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.column img {
  width: 100%;
  height: 250px;
  display: block;
  margin: 0;
  padding: 0;
  border: none;
}

@media (max-width: 768px) {
  .row {
    max-width: 90vw;
    max-height: 90vh;
  }
}
</style>
