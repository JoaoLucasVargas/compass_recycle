<template>
  <div>
    <div class="clock-date" >
        <p class="clock">{{hora}}</p>
        <div class="date">{{ date }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ApiClock",
  data() {
    return {
      api_key: "4ab53cc4e8c31cba8045819f6317e698",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      lang: "pt_br",
      weather: {},
      hora: this.formData(),
      date: "Terça-feira, 10 de Janeiro de 2020"
      
    };
  },
  created() {
    this.cordenadas();
    this.date = this.dateBuilder();
  },
  methods: {
    formData() {
        let data = new Date();
        let hora = data.getHours();
        let minutos = data.getMinutes();
        return `${hora}:${minutos} `
    },
    cordenadas() {
   
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            this.fetchWeather(
              position.coords.latitude,
              position.coords.longitude
            );
          },
          (error) => {
            console.log(error);
          }
        );
      } else {
        console.log("Geolocation is not supported by this browser.");
      }
    },

    fetchWeather(lat, long) {
      fetch(
        `${this.url_base}weather?lat=${lat}&lon=${long}&lang=${this.lang}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setResults);
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julio",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro",
      ];
      let days = [
        "Domingo",
        "Segunda",
        "Terça",
        "Quarta",
        "Quinta",
        "Sexta",
        "Sábado",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day}, ${date} de ${month} de ${year}`;
      
    },
  },
};
</script>

<style lang="scss" scoped>
@import "./ApiClock.scss";
</style>
