<template>
  <!-- Root Div  -->
  <div>
    <h1>Welcome</h1>
    <div v-for="place in places" :key="place.id">
      {{ place }}
    </div>
    <!-- Create New Place  -->
    <dialog id="place-new">
      <p>Add a new place:</p>
      <form>
        <p>
          Place name:
          <input type="text" v-model="newPlaceParams.name" placeholder="name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="newPlaceParams.address" placeholder="address" />
        </p>
      </form>
      <button v-on:click="placesNew()">create new place</button>
    </dialog>
    <button v-on:click="modalNewPlace()">Add a new place</button>

    <!-- </dialog> -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: {},
      newPlaceParams: {},
    };
  },
  methods: {
    placesIndex: function () {
      axios.get("http://localhost:3000/places.json").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },
    placesNew: function () {
      console.log(this.newPlaceParams);
      axios.post("http://localhost:3000/places.json", this.newPlaceParams).then((response) => {
        console.log(response.data);
        this.places << response.data;
      });
      this.newPlaceParams.name = null;
      this.newPlaceParams.address = null;
    },
    modalNewPlace: function () {
      document.getElementById("place-new").showModal();
    },
  },
  created: function () {
    this.placesIndex();
  },
};
</script>
