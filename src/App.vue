<template>
  <div id="app" class="hh">
    <get-city v-on:thisCity="takeCity" />
    <widget v-bind:CityName="CityName" />
  </div>
</template>

<!-- SCRIPTS -->
<script>
import getCity from "./components/getCity.vue";
import Widget from "./components/Widget.vue";

export default {
  components: {
    getCity,
    Widget,
  },
  name: "App",
  data() {
    return {
      CityName: "",
    };
  },

  methods: {
    takeCity(CityName) {
      this.CityName = CityName;
    },
  },
};
</script>

<style></style>
