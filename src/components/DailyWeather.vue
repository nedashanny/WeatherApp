<script>
    export default{
        data(){
            return{
                dayIsActive:true,
                nightIsActive:false,
            }
        },
        props:{
            title:String,
            forecasts:Array
        },
        methods:{
            formateDate(utcString){
                var date=new Date(utcString);
                return date.toDateString()
            },
            dayNight(){
                this.dayIsActive = !this.dayIsActive
                this.nightIsActive = !this.nightIsActive
            },
        }
    }
</script>

<template>
    <p class="fs-3 text-capitalize fw-bold mt-1">{{ title }}</p>
    <ul class="nav nav-pills card-header-pills">
      <li class="nav-item">
        <button class="nav-link" v-on:click="dayNight()" :class="{active:dayIsActive}">Days</button>
      </li>
      <li class="nav-item">
        <button class="nav-link" v-on:click="dayNight()" :class="{active:nightIsActive}">Nights</button>
      </li>
    </ul>

    <div class="w-100 overflow-scroll m-1" style="white-space: nowrap;" v-if="dayIsActive">
      <div class="card m-1 d-inline-block" style="width: 13rem;" v-for="(forecast,index) in forecasts" :key="index">
        <div class="card-header">{{ formateDate(forecast.Date) }}</div>
        <img :src="`src/assets/icons/${forecast.Day.Icon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body">
          <h5 class="card-title">{{forecast.Day.IconPhrase}}</h5>
          <p class="fs-4">{{ forecast.Temperature.Minimum.Value+"\u00B0" +" "+ forecast.Temperature.Minimum.Unit + " _ " +forecast.Temperature.Maximum.Value+"\u00B0" +" "+ forecast.Temperature.Maximum.Unit }}</p>
        </div>
      </div>
    </div>

    <div class="w-100 overflow-scroll m-1" style="white-space: nowrap;" v-if="nightIsActive">
      <div class="card bg-dark m-1 d-inline-block" style="width: 13rem;" v-for="(forecast,index) in forecasts" :key="index">
        <div class="card-header text-white">{{ formateDate(forecast.Date) }}</div>
        <img :src="`src/assets/icons/${forecast.Night.Icon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body text-white">
          <h5 class="card-title">{{forecast.Night.IconPhrase}}</h5>
          <p class="fs-4">{{ forecast.Temperature.Minimum.Value+"\u00B0" +" "+ forecast.Temperature.Minimum.Unit + " _ " +forecast.Temperature.Maximum.Value+"\u00B0" +" "+ forecast.Temperature.Maximum.Unit }}</p>
        </div>
      </div>
    </div>
</template>