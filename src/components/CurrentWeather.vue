<script>
    export default{
        props:{
            title:String,
            weather:Array,
        },
        methods:{
          cardBodyTextColor(isDay){
            return isDay?'black':'white'
          }
        }
    }
</script>
<template>
  <!-- title -->
    <p class="fs-3 text-capitalize fw-bold">{{ title }}</p>
    <!-- container -->
    <div class="w-100 overflow-scroll" style="white-space: nowrap;">
      <!-- cards -->
      <div class="card m-1 d-inline-block" style="width: 13rem;" v-for="(weather,index) in weather" :key="index" :class="{'bg-dark':!weather.IsDayTime}">
        <img :src="`src/assets/icons/${weather.WeatherIcon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body" :style="{'color':cardBodyTextColor(weather.IsDayTime)+'!important'}">
          <h5 class="card-title fs-6">{{weather.WeatherText}}</h5>
          <p class="fs-4">{{ weather.Temperature.Metric.Value+"\u00B0" + " " + weather.Temperature.Metric.Unit}}</p>
          <a :href="weather.Link" class="btn btn-primary text-capitalize">view website</a>
        </div>
      </div>
    </div>
</template>

<style scoped>
.overflow-scroll::-webkit-scrollbar{
display: none;
}
</style>