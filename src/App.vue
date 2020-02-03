<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          class="search-bar" 
          type="text" 
          id="searcher" 
          placeholder="Pesquisar Cidade" 
          v-model= "query"
          @keypress= "fetchWeather"
        />

      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">

        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">Segunda, 03 de Fevereiro de 2020</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>

      </div>

    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      apiKey: '343705ebc2c1e42c4aca1b22958d8868',
      urlBase: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {}
    }
  },

  methods: {
    fetchWeather (e){
      if (e.keyCode == 13){
        fetch(`${this.urlBase}weather?q=${this.query}&units=metric&APPID=${this.apiKey}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },

    setResults (results){
      this.weather = results;
    },

    getDate (){
      let d = new Date();
      let dias = ["Domingo", "Segunda-Feira", "Terça-Feira", "Quarta-Feira", "Quinta-Feira", "Sexta-Feira", "Sabado"];
      let meses = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];

      let dia = dias[d.getDay()];
      let data = d.getDate();
      let mes = meses[d.getMonth()];
      let ano = d.getFullYear();

      return `${dia}, ${data} de ${mes} de ${ano}`;
    }

  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }


  body{
    font-family: 'montserrat', sans-serif;
  }

  main{
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
  }


  #app{
    background-image: url('./assets/img/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm{
    background-image: url('./assets/img/warm-bg.jpg');
  }

  #searcher{
    width: 100%;
    height: 30px;
  }


  .search-box .search-bar{
    display: block;
    width: 100%;
    padding: 15px;
    color: #333;
    font-size: 20px;
    border: none;
    appearance: none;
    outline: none;
    background: none;
    background-color: rgba(255,255,255,0.5);
    box-shadow: 0px 0px 8px rgba(0,0,0,0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus{
    background-color: rgba(255,255,255,0.9);
    box-shadow: 0px 0px 16px rgba(0,0,0,0.5);
    border-radius: 16px 0px 16px 0px;
  }

  .location-box{
    margin-top: 30px;
    margin-bottom: 30px;
  }

  .location-box .location{
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
  }

  .location-box .date{
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
  }

  .weather-box{
    text-align: center;
  }

  .weather-box .temp{
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 120px;
    font-weight: 1000;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.25);
    border-radius: 16px;
    margin-bottom: 15px;
    box-shadow: 3px 6px rgba(0,0,0,0.25);
  }

  .weather-box .weather{
    padding: 10px 25px;
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
  }

  @media only screen and (max-width: 600px){
    .weather-box .temp{
      display: block;
      border-radius: 16px 16px 0px 0px;
    }
    .weather-box .weather{
      padding: 10px 25px;
      color: #fff;
      font-size: 48px;
      font-weight: 700;
      text-shadow: 3px 6px rgba(0,0,0,0.25);
      background-color: rgba(255,255,255,0.25);
      border-radius: 0px 0px 16px 16px;
      box-shadow: 3px 6px rgba(0,0,0,0.25);
    }
  }

</style>
