<template >
  <div :class="{
      'home': forecast !== 'other',
      'home sunny': forecast === 'Sunny',
      'home rain': forecast === 'Rain',
      'home snow': forecast === 'Snow', 
      'home clouds': forecast === 'Clouds',
      'home clear': forecast === 'Clear',
      'home thunderstorm': forecast === 'Thunderstorm',
      'home tornado': forecast === 'Tornado',

    }">
    <div class="search-box">
      <input type="text" class="search-text" placeholder="Search..." @keypress="keypress" v-model="queryString">
      <p v-if="isLoading">Loading...</p>
    </div>

    <div class="weather-box" v-if="!isError && weather !== null ">
      <div class="location">
        <h3>{{weather.name}}, {{weather.sys.country}}</h3>
      </div>
      <div class="date">
        <h4>{{todayDate}}</h4>
      </div>  
      <div class="temp">
        <div>
          <h4>{{weather.main.temp}}˚ celcius</h4>
          <p>Feels like {{weather.main.feels_like}}</p>
        </div>
      </div>
      <div class="weather">
        <h3>{{ forecast }}</h3>
        <p>{{weather.weather[0].description}}</p>
      </div>
    </div>

    <div class="weather-box" v-else>
      <h3>{{ isError.message }}</h3>
    </div>
  </div>

  <Footer />

</template>

<script>
// @ is an alias to /src
import Footer from '@/views/Footer.vue'
import dayjs from 'dayjs'

export default {
  name: 'Home',
  components: { Footer },

  data(){
    return{
      forecast: 'Clear',
      api_key: process.env.VUE_APP_KEY,
      base_url: process.env.VUE_APP_BASE_URL,
      queryString: 'Calgary, alberta',
      weather: null,
      isLoading: false,
      isError: '',
      todayDate: dayjs().format("MMMM DD YYYY")
    }
  },

  methods:{
    keypress(e){
      if( e.key === "Enter"){
        this.fetchWeather()
      }
    },
    fetchWeather(){
      this.isLoading = true
      fetch(`${this.base_url}weather?q=${this.queryString}&units=metric&APPID=${this.api_key}`)
          .then( res => res.json())
          .then( data => {
            if( data.cod === "404" ){
              this.isError = data
              this.isLoading = false
            }else{
              this.isError = null
              this.weather =  data
              this.isLoading = false
              this.forecast = this.weather.weather[0].main
            }
          })
          .catch( err => { console.log( err )})
    },
  },

  mounted(){
   this.isLoading = true
   this.fetchWeather()
  }

}
</script>


<style lang="scss">
  .home{
    position: relative;
    height: 100%;
    transition: all 3s ease-in-out;

    &::before{
      position: absolute;
      content: '';
      top: 0;
      left: 0;
      height: 100%;
      width: 100%;
      background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.65));
    }

    .search-box{
      position: absolute;
      width: 100%;
      padding: 3em;

      p{
        padding: 1em;
        z-index: 1;
        font-size: 1.4em;
      }

      input[type=text]{
        appearance: none;
        border: none;
        outline: none;
        padding: 0.5em 1em;
        font-size: 1em;
        border-radius: 0 8px 0 8px;
        width: 90%;
      }
    }

    .weather-box{
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: center;

      .location{
        color: black;
        font-size: 1.5em;
        text-shadow: 2px 3px rgba(0,0,0,0.25);
      }

      .date{
        color: black;
      }

      .temp{
        margin-top: 1em;
        padding: 0.5em 1em;
        display: flex;
        flex-direction: column;
        justify-content: center;
        
        div{
           font-size: 2em;
          padding: 1em;
          background-color: rgba(0,0,0,0.65);
          border-radius: 5px;
          color: white;
          z-index: 1;

          p{
            font-size: 0.5em;
          }
        }
       
      }
    }

    .weather{
      margin-top: 1em;
      color: white;
      z-index: 1;
      font-size: 1.5em;

      p{
        font-size: 0.5em;
      }
    }

    &.sunny{
      background-image: url('../assets/sunny.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: all 3s ease-in-out;
    }

    &.rain{
      background-image: url('../assets/rain.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: all 3s ease-in-out;
    }

    &.snow{
      background-image: url('../assets/snow.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: all 3s ease-in-out;
    }

     &.clouds{
      background-image: url('../assets/clouds.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: all 3s ease-in-out;
    }

     &.clear{
      background-image: url('../assets/clear.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: all 3s ease-in-out;
    }

    &.thunderstorm{
      background-image: url('../assets/thunderstorm.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: all 3s ease-in-out;
    }

     &.tornado{
      background-image: url('../assets/tornado.jpg');
      background-size: cover;
      background-repeat: no-repeat;
      transition: all 3s ease-in-out;
    }
  }
</style>
