<template>
  <main>
    <div class="weather-wrap" v-if="typeof data.main != 'undefined'">
      <div
        id="app1"
        class="info"
        :class="{ maxTemp: maxTemp, minTemp: minTemp }"
      >
        <div class="location-temp">
          <p class="location">{{ query }}</p>
          <p class="temp">{{ Math.round(data.main.temp) }}°C</p>
        </div>
        <div class="weather-wind">
          <p class="weather">{{ data.weather[0].main }}</p>
          <p class="wind">{{ data.wind.speed }} м/с</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  props: {
    CityName: {
      type: String,
    },
  },
  data() {
    return {
      data: [],
      maxTemp: 0,
      minTemp: 0,
      query: "",
    };
  },
  methods: {
    async search() {
      //функция для парса данных
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&APPID=b716e8d1dec234250048fd364cfa9c6f`
      );
      const json = await response.json();
      console.log(json);
      this.data = json;
      //Смена цвета при определенной температуре
      try {
        if (Math.round(json.main.temp) > 20) {
          this.maxTemp = 1;
          this.minTemp = 0;
        } else if (Math.round(json.main.temp) < 0) {
          this.minTemp = 1;
          this.maxTemp = 0;
        } else {
          this.minTemp = 0;
          this.maxTemp = 0;
        }
      } catch (e) {
        //Обработка ошибки если города нет
        if (this.query != undefined) {
          alert("Информации по городу " + this.query + " Нет");
        }
      }
      console.log(this.query);
    },
  },
  watch: {
    query(weather) {
      sessionStorage.query = weather; //получение данных селекта
      this.search(); //после получения выполняем функцию парса
    },
    CityName() {
      //закидываем в переменную значение полученного города
      this.query = this.CityName;
    },
  },
  mounted() {
    setInterval(() => {
      this.search();
    }, 60000);
    this.query = sessionStorage.query;
  },
};
</script>

<style>
.location-box .location {
  font-size: 32px;
  text-align: center;
}
.info {
  position: fixed;
  top: 30%;
  display: flex;
  justify-content: space-between;
  width: 200px;
  border: 1px solid black;
  border-radius: 15px;
  padding: 0 30px;
}
.info .temp {
  font-size: 30px;
  margin: 0;
  font-weight: bold;
}
.weather-wind {
  margin-top: 12px;
}
.maxTemp {
  position: fixed;
  top: 30%;
  display: flex;
  justify-content: space-between;
  width: 200px;
  border: 1px solid black;
  border-radius: 15px;
  padding: 0 30px;
  background-color: #ffffcc;
}
.minTemp {
  position: fixed;
  top: 30%;
  display: flex;
  justify-content: space-between;
  width: 200px;
  border: 1px solid black;
  border-radius: 15px;
  padding: 0 30px;
  background-color: #ccffff;
}
</style>
