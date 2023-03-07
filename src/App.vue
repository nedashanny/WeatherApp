<script>
  import axios from "axios";
  export default{
    data(){
      return{
        ipAdress:null,
        currentconditions:null,
        forecastsHourly:null,
      }
    },
    beforeMount(){
      // get user ip
      axios.get('https://api.ipify.org?format=json')
          .then(response=>{
            this.ipAdress=response.data.ip
          })
    },
    mounted(){
      // find user location
      axios.get('http://dataservice.accuweather.com/locations/v1/cities/ipaddress?q='+this.ipAdress+'&apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
          .then(response=>{
                axios.get('http://dataservice.accuweather.com/currentconditions/v1/'+response.data.Key+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
                    .then(response=>{
                      this.currentconditions=response
                    })
                axios.get('http://dataservice.accuweather.com/forecasts/v1/hourly/12hour/'+response.data.Key+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
                    .then(response=>{
                      this.forecastsHourly=response.data
                      console.log(response)
                    })    
          })
    },
    methods:{
      formateTime(utcString){
        var date=new Date(utcString);
        return date.toLocaleTimeString()
      },
      
      }
    }
</script>

<template>
  <nav class="navbar bg-body-tertiary">
    <div class="container">
      <a class="navbar-brand">Accu</a>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Location" aria-label="Location">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </nav>
  <div class="container">
    <!-- current weather -->
    <p class="fs-3 text-capitalize fw-bold">current</p>
    <div v-if="currentconditions!=null" class="card m-1" style="width: 10rem;">
      <img :src="`src/assets/icons/${currentconditions.data[0].WeatherIcon}-s.png`" class="card-img-top mt-1" alt="...">
      <div class="card-body">
        <h5 class="card-title">{{ currentconditions.data[0].WeatherText }}</h5>
        <p class="fs-4">{{ currentconditions.data[0].Temperature.Metric.Value + " "+  currentconditions.data[0].Temperature.Metric.Unit}}</p>
        <a :href="currentconditions.data[0].Link" class="btn btn-primary text-capitalize">view website</a>
      </div>
    </div>
    <!-- 12 hours forcast weather -->
    <p class="fs-3 text-capitalize fw-bold">hourly</p>
    <div class="w-100 overflow-scroll" style="white-space: nowrap;" v-if="forecastsHourly!=null">
      <div class="card m-1 d-inline-block" style="width: 10rem;" v-for="(forecast,index) in forecastsHourly" :key="index">
        <div class="card-header">{{ formateTime(forecast.DateTime) }}</div>
        <img :src="`src/assets/icons/${forecast.WeatherIcon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body">
          <h5 class="card-title">{{forecast.IconPhrase}}</h5>
          <p class="fs-4">{{ forecast.Temperature.Value + " " + forecast.Temperature.Unit}}</p>
        </div>
      </div>
    </div>
    <!-- 5days forcast weather -->
    <p class="fs-3 text-capitalize fw-bold mt-1">daily</p>
    <div class="w-100 overflow-scroll" style="white-space: nowrap;">
      <div class="card m-1 d-lg-inline-block" style="width: 10rem;">
        <div class="card-header">Sunday</div>
        <img src="./assets/icons/1-s.png" class="card-img-top mt-1" alt="...">
        <div class="card-body">
          <h5 class="card-title">Cloudy</h5>
          <p class="fs-4">17 C</p>
        </div>
      </div>
    </div>
  </div>
  
</template>