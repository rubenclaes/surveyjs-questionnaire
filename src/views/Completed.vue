<template>
  <div class="container">
    <div class="col-lg-3 centered">
      <img src="../assets/logo.png" />
    </div>
    <div class="col-lg-9 jumbotron">
      <h1>Resultaten</h1>
      <p>Bedankt voor het invullen van de vragenlijst.</p>
      <p>Je score is {{ absoluteScore.toString() }}: {{ text }}</p>

      <p></p>
      <p></p>
    </div>

    <div class="container">
      <canvas ref="canvas" width="600" height="100"></canvas>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.css";

import { PolarArea } from "vue-chartjs";
import { router } from "../App";

export default {
  name: "Page2",
  extends: PolarArea,
  mounted() {
    this.renderChart(
      {
        labels: ["Extravert", "Introvert"],
        datasets: [
          {
            label: "Extravert vs Introvert",
            data: [this.extravertScore, this.introvertScore],
            backgroundColor: [
              "rgba(255, 99, 132, 0.2)",
              "rgba(54, 162, 235, 0.2)",
            ],
            borderColor: ["rgba(255, 99, 132, 1)", "rgba(54, 162, 235, 1)"],
            borderWidth: 1,
          },
        ],
      },
      { responsive: true, maintainAspectRatio: false }
    );
  },
  data() {
    return {
      extravertScore: 0,
      introvertScore: 0,
      absoluteScore: 5,
      text: "",
    };
  },

  created() {
    if (
      this.$route.params.extravertScore &&
      this.$route.params.introvertScore &&
      this.$route.params.absoluteScore
    ) {
      this.extravertScore = this.$route.params.extravertScore;
      this.introvertScore = this.$route.params.introvertScore;
      this.absoluteScore = parseInt(this.$route.params.absoluteScore);
      this.text = this.$route.params.text;
    }
  },
  methods: {},
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
