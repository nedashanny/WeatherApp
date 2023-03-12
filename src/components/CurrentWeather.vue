<script>
    export default{
        props:{
            title:String,
            weather:Array,
        }
    }
</script>
<template>
    <p class="fs-3 text-capitalize fw-bold">{{ title }}</p>
    <div class="w-100 overflow-scroll" style="white-space: nowrap;">
      <div class="card m-1 d-inline-block" style="width: 13rem;" v-for="(weather,index) in weather" :key="index" :class="{'bg-dark':!weather.IsDayTime}">
        <img :src="`src/assets/icons/${weather.WeatherIcon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body" :class="{'text-white':!weather.IsDayTime}">
          <h5 class="card-title">{{weather.WeatherText}}</h5>
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