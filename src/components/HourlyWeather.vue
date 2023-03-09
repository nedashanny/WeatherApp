<script>
    export default{
        props:{
            title:String,
            forecasts:Array
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
    <p class="fs-3 text-capitalize fw-bold">{{ title }}</p>
    <div class="w-100 overflow-scroll" style="white-space: nowrap;">
      <div class="card m-1 d-inline-block" style="width: 13rem;" v-for="(forecast,index) in forecasts" :key="index" :class="{'bg-dark':!forecast.IsDaylight}">
        <div class="card-header" :class="{'text-white':!forecast.IsDaylight}">{{ formateTime(forecast.DateTime) }}</div>
        <img :src="`src/assets/icons/${forecast.WeatherIcon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body" :class="{'text-white':!forecast.IsDaylight}">
          <h5 class="card-title">{{forecast.IconPhrase}}</h5>
          <p class="fs-4">{{ forecast.Temperature.Value + " " + forecast.Temperature.Unit}}</p>
        </div>
      </div>
    </div>
</template>