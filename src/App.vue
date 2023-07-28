<script>
import axios from "axios";
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return this.city;
    },
  },
  methods: {
    getWeather: function (event) {
      // `this` внутри методов указывает на экземпляр Vue
      // alert("Привет, " + "!");
      // `event` — нативное событие DOM
      if (this.city.trim().length < 2) {
        this.error = "Напишите больше символов";
        return false;
      }
      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=bcf966b83c6eb0a9025cee074d2f55fc`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input
      type="text"
      v-on:input="this.city = $event.target.value"
      placeholder="Введите город"
    />
    <button v-if="city != ''" v-on:click="getWeather">Получить погоду</button>
    <!-- не показывать кнопкуесли пустая строка -->
    <button disable v-else>Введите название города</button>
    <p class="error">{{ error }}</p>
    <p v-if="info != null">Температура: {{ info.main.temp }}</p>
    <p v-if="info != null">Ощущается как: {{ info.main.feels_like }}</p>
    <p v-if="info != null">
      Минимум: {{ info.main.temp_min }} Максимум: {{ info.main.temp_max }}
    </p>
    <p v-if="info != null">Влажность воздуха {{ info.main.humidity }}</p>
    <!-- показывать если пустая -->
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  color: #fff;
}
.wrapper h1 {
  margin-top: 50px;
}
.wrapper p {
  margin-top: 50px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fff;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}
.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}
.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
