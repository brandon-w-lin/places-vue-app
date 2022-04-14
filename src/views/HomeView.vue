<template>
  <!-- Root Div  -->
  <div>
    <h1>Welcome</h1>
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
      <button v-on:click="modalPlaceNewClose()">close</button>
    </dialog>
    <button v-on:click="modalPlaceNewOpen()">Add a new place</button>

    <!-- Index Place  -->
    <h2>See places below:</h2>
    <div v-for="place in places" :key="place.id">
      {{ place.name }}
      <!-- Show Place  -->
      <button v-on:click="modalPlaceInfoOpen(place)">More Info</button>
      <dialog id="place-show">
        <form>
          <p>
            Name:
            <input type="text" v-model="currentPlace.name" />
          </p>
          <p>
            Address:
            <input type="text" v-model="currentPlace.address" />
          </p>
        </form>

        <button v-on:click="placeUpdate(currentPlace)">Update</button>
        <button v-on:click="placeDestroy(currentPlace)">Delete</button>
        <button v-on:click="modalPlaceInfoClose()">Close</button>
      </dialog>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: {},
      newPlaceParams: {},
      editPlaceParams: {},
      currentPlace: {},
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
      axios
        .post("http://localhost:3000/places.json", this.newPlaceParams)
        .then((response) => {
          console.log(response.data);
          this.places << response.data;
        })
        .catch((error) => console.log(error.response));
      this.newPlaceParams.name = null;
      this.newPlaceParams.address = null;
      this.placesIndex();
    },
    placeUpdate: function (place) {
      this.currentPlace = place;
      axios
        .patch("http://localhost:3000/places/" + this.currentPlace.id + ".json", this.editPlaceParams)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => console.log(error.response));
      this.placesIndex();
      this.modalPlaceInfoClose();
    },
    placeDestroy: function (place) {
      this.currentPlace = place;
      axios
        .delete("http://localhost:3000/places/" + this.currentPlace.id + ".json")
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => console.log(error.response));
      this.placesIndex();
      this.modalPlaceInfoClose();
    },
    modalPlaceNewOpen: function () {
      document.getElementById("place-new").showModal();
    },
    modalPlaceNewClose: function () {
      document.getElementById("place-new").close();
    },
    modalPlaceInfoOpen: function (place) {
      this.currentPlace = place;
      document.getElementById("place-show").showModal();
    },
    modalPlaceInfoClose: function () {
      document.getElementById("place-show").close();
    },
  },
  created: function () {
    this.placesIndex();
  },
};
</script>
