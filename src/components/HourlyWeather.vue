<script>
    //from https://chartscss.org/
    import 'charts.css';
    export default{
        data(){
          return{
            chart:'',
          }
        },
        props:{
            title:String,
            forecasts:Array
        },
        mounted(){
          this.createChart(this.forecasts)
        },
        beforeUpdate(){
          this.createChart(this.forecasts)
        },
        methods:{
          formateTime(utcString){
            var date=new Date(utcString);
            return date.toLocaleTimeString()
          },
          createElementFromString(){
            document.createElement('')
          },
          createChart(forecasts){
            this.chart=''
            forecasts.forEach((item,i) => {
              if(i!=forecasts.length-1){
                this.chart += `<tr><td style="--start: ${item.Temperature.Value/100}; --size: ${forecasts[i+1].Temperature.Value/100}"> <span class="data"> ${forecasts[i+1].Temperature.Value+"\u00B0"} </span> </td></tr>`
              }
            });
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
      <div class="card m-1 d-inline-block" style="width: 13rem;" v-for="(forecast,index) in forecasts" :key="index" :class="{'bg-dark':!forecast.IsDaylight}">
        <div class="card-header" :class="{'text-white':!forecast.IsDaylight}">{{ formateTime(forecast.DateTime) }}</div>
        <img :src="`src/assets/icons/${forecast.WeatherIcon}-s.png`" class="card-img-top mt-1" alt="...">
        <div class="card-body" :class="{'text-white':!forecast.IsDaylight}">
          <h5 class="card-title fs-6">{{forecast.IconPhrase}}</h5>
          <p class="fs-4">{{ forecast.Temperature.Value +"\u00B0"+ " " + forecast.Temperature.Unit}}</p>
        </div>
      </div>
    </div>

    <p class="fs-2 text-capitalize fw-bold mt-2">Hourly Temperature Chart</p>
    <table class="charts-css line show-10-secondary-axes" id="my-chart" style="width: 100%; height: 10rem;">
      <tbody v-html="chart"></tbody>
    </table>
</template>