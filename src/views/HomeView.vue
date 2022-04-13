<template>
  <div>
    <h1>Welcome</h1>
    <div v-for="place in places" :key="place.id">{{ place }}</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: {},
    };
  },
  methods: {
    placesIndex: function () {
      axios.get("http://localhost:3000/places.json").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },
  },
  created: function () {
    this.placesIndex();
  },
};
</script>
