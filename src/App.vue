<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <star-rating
      class="star"
      :increment="0.5"
      text-class="custom-text"
      @rating-selected="setRating"
    ></star-rating>
    <div v-html="rating"></div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import StarRating from "vue-star-rating";

export default {
  name: "app",
  components: {
    HelloWorld,
    StarRating,
  },
  data() {
    return {
      rating: "No Rating Selected",
      currentRating: "No Rating",
      currentSelectedRating: "No Current Rating",
      boundRating: 3,
    };
  },
  methods: {
    setRating: function(rating) {
      this.rating = "You have Selected: " + rating + " stars";
    },
    showCurrentRating: function(rating) {
      this.currentRating =
        rating === 0
          ? this.currentSelectedRating
          : "Click to select " + rating + " stars";
    },
    setCurrentSelectedRating: function(rating) {
      this.currentSelectedRating = "You have Selected: " + rating + " stars";
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.star {
  display: flex;
  justify-content: center;
}
.custom-text {
  font-weight: bold;
  font-size: 1.9em;
  border: 1px solid #cfcfcf;
  padding-left: 10px;
  padding-right: 10px;
  border-radius: 5px;
  color: #999;
  background: #fff;
}
</style>
