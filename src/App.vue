<template>
  <div id="app">
    <full-page :options="options" id="fullpage" v-if="isLoaded">
      <div
        v-for="showerThought in showerThoughts"
        :key="showerThought.data.id"
        class="section"
      >
        <div class="container">
          <div class="content">
            <img src="./assets/logo.png" alt="Logo" />

            <div class="quote-symbol">&ldquo;</div>
            <p class="quote">
              <a :href="showerThought.data.url">{{
                showerThought.data.title
              }}</a>
            </p>
            <div class="details">
              <div class="details-author">
                Author: {{ showerThought.data.author }}
              </div>
              <div class="details-stats">
                {{ showerThought.data.ups }} Upvote |
                {{ showerThought.data.num_comments }} Comments
              </div>
            </div>
          </div>
        </div>
      </div>
    </full-page>
    <div class="container" v-else>
      <loading-spinner />
    </div>
  </div>
</template>

<script>
import LoadingSpinner from "./components/LoadingSpinner";

export default {
  name: "App",

  components: {
    LoadingSpinner,
  },

  created() {
    fetch("https://www.reddit.com/r/Showerthoughts.json?limit=10")
      .then((res) => res.json())
      .then((data) => {
        this.showerThoughts = data.data.children;
        this.showerThoughts.shift();
        this.showerThoughts.shift();
        this.isLoaded = true;
      });
  },

  data() {
    return {
      isLoaded: false,
      showerThoughts: [],
      options: {
        scrollBar: false,
        sectionsColor: [
          "#41b883",
          "#ff5f45",
          "#0798ec",
          "#fec401",
          "#1bcee6",
          "#ee1a59",
          "#2c3e4f",
          "#ba5be9",
          "#4DB3FF",
          "#FF6633",
          "#FF3380",
        ],
      },
    };
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Sriracha&display=swap");

#app {
  font-family: sans-serif;
  color: white;
}

body {
  margin: 0;
  padding: 0;
}

h3 {
  margin: 0;
}

.container {
  margin: auto;
  max-width: 800px;
  display: grid;
  place-items: center;
  height: 100vh;
}

.section {
  text-align: center;
}

.quote-symbol {
  font-size: 64px;
  line-height: 0;
  margin-top: 50px;
  color: white;
}

.quote a {
  text-decoration: none;
  font-family: "Sriracha", cursive;
  color: white;
  font-size: 32px;
  line-height: 1.5;
  margin-left: 15px;
  margin-right: 15px;
}

.quote a:hover {
  color: #edf2f7;
}

.details-stats {
  margin-top: 4px;
}
</style>
