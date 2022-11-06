<template>
  <div class="search-box">
    <select class="city" v-on:change="changedCity">
      <option value="none">Выберите страну</option>
      <option v-bind:value="city.capital" v-for="city of cities" :key="city.id">
        {{ city.name }}
      </option>
    </select>
  </div>
</template>
<script>
export default {
  data() {
    return {
      cities: [],
    };
  },
  methods: {
    async GetCity() {
      const getcity = await fetch(
        "https://restcountries.com/v2/all?fields=name,capital"
      ); //получаем данные городов
      const jsoncity = await getcity.json();
      this.cities = jsoncity;
    },
    changedCity(event) {
      //$emit из дочернего в родительский, потому что из дочернего в дочерний нельзя
      this.$emit("thisCity", event.target.value);
    },
  },
  mounted() {
    this.GetCity();
  },
};
</script>

<style></style>
