<script>
  import axios from "axios";
  export default{
    data(){
      return{
        ipAdress:null,
        currentconditions:null,
        forecastsHourly:null,
        forecastsDaily:null,
        dayIsActive:true,
        nightIsActive:false,
        city:'',
        locations:null,
        showLocations:true,
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
                 this.displayWeather(response.data.Key)        
          })
    },
    methods:{
      formateTime(utcString){
        var date=new Date(utcString);
        return date.toLocaleTimeString()
      },

      formateDate(utcString){
        var date=new Date(utcString);
        return date.toDateString()
      },

      dayNight(){
        this.dayIsActive = !this.dayIsActive
        this.nightIsActive = !this.nightIsActive
      },
      displayWeather(location){
        axios.get('http://dataservice.accuweather.com/currentconditions/v1/'+location+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
              .then(response=>{
                  this.currentconditions=response
              })
        axios.get('http://dataservice.accuweather.com/forecasts/v1/hourly/12hour/'+location+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
              .then(response=>{
                  this.forecastsHourly=response.data
              })    
        axios.get('http://dataservice.accuweather.com/forecasts/v1/daily/5day/'+location+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
              .then(response=>{
                  this.forecastsDaily=response.data.DailyForecasts
              })  
        this.showLocations=true      
      },
      search(){
        axios.get('https://dataservice.accuweather.com/locations/v1/cities/search?q='+this.city+'&apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
                .then( (response)=> {
                  this.locations=response.data
                })
        this.showLocations=false       
      },
      
      }
    }
</script>

<template>
  <nav class="navbar bg-body-tertiary">
    <div class="container">
      <a class="navbar-brand">Accu</a>
      <form @submit.prevent="search()" class="d-flex" role="search">
        <input v-model="city" class="form-control me-2" type="search" placeholder="Location" aria-label="Location">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </nav>
  <div class="container">
    <!-- locations -->
    <ul class="list-group mt-1" :class="{'d-none':showLocations}">
      <li class="list-group-item" v-for="location in locations" :key="location" @click="displayWeather(location.Key)">{{ location.Country.LocalizedName+','+location.LocalizedName }}</li>
    </ul>
    <!-- current weather -->
    <p class="fs-3 text-capitalize fw-bold">current</p>
    <div v-if="currentconditions!=null" class="card m-1" style="width: 13rem;">
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
      <div class="card m-1 d-inline-block" style="width: 13rem;" v-for="(forecast,index) in forecastsHourly" :key="index">
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
    <ul class="nav nav-pills card-header-pills">
      <li class="nav-item">
        <button class="nav-link" v-on:click="dayNight()" :class="{active:dayIsActive}">Days</button>
      </li>
      <li class="nav-item">
        <button class="nav-link" v-on:click="dayNight()" :class="{active:nightIsActive}">Nights</button>
      </li>
    </ul>

    <div class="w-100 overflow-scroll m-1" style="white-space: nowrap;" v-if="dayIsActive">
      <div class="card m-1 d-inline-block" style="width: 13rem;" v-for="(forecast,index) in forecastsDaily" :key="index">
        <div class="card-header">{{ formateDate(forecast.Date) }}</div>
        <img :src="`src/assets/icons/${forecast.Day.Icon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body">
          <h5 class="card-title">{{forecast.Day.IconPhrase}}</h5>
          <p class="fs-4">{{ forecast.Temperature.Minimum.Value +" "+ forecast.Temperature.Minimum.Unit + " _ " +forecast.Temperature.Maximum.Value +" "+ forecast.Temperature.Maximum.Unit }}</p>
        </div>
      </div>
    </div>

    <div class="w-100 overflow-scroll m-1" style="white-space: nowrap;" v-if="nightIsActive">
      <div class="card bg-dark m-1 d-inline-block" style="width: 13rem;" v-for="(forecast,index) in forecastsDaily" :key="index">
        <div class="card-header text-white">{{ formateDate(forecast.Date) }}</div>
        <img :src="`src/assets/icons/${forecast.Night.Icon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body text-white">
          <h5 class="card-title">{{forecast.Night.IconPhrase}}</h5>
          <p class="fs-4">{{ forecast.Temperature.Minimum.Value +" "+ forecast.Temperature.Minimum.Unit + " _ " +forecast.Temperature.Maximum.Value +" "+ forecast.Temperature.Maximum.Unit }}</p>
        </div>
      </div>
    </div>
  </div>
  
</template>