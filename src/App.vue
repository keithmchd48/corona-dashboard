<template>
  <div id="app">
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <h1 class="text-center" style="color: #52b883">COVID-19 Data Visualization</h1>
        </div>
      </div>
      <!--ROW 1-->
      <div class="row mt-5">
        <div class="col-xs-12 col-sm-12 col-md-12" v-if="arrPositive.length > 0">
          <h2>Positive</h2>
          <LineCharts :chartData="arrPositive" :options="chartOptions" label="Positive" :chartColors="positiveChartColors"></LineCharts>
        </div>
      </div>
      <!--ROW 2-->
      <div class="row mt-5">
        <div class="col-xs-12 col-sm-12 col-md-12" v-if="arrHospitalized.length > 0">
          <h2>Hospitalized</h2>
          <LineCharts :chartData="arrHospitalized" :options="chartOptions" label="Hospitalized" :chartColors="hospitalizedChartColors"></LineCharts>
        </div>
      </div>
      <!--ROW 3-->
      <div class="row mt-5">
        <div class="col-xs-12 col-sm-12 col-md-12" v-if="arrInIcu.length > 0">
          <h2>In ICU</h2>
          <LineCharts :chartData="arrInIcu" :options="chartOptions" label="In ICU" :chartColors="inIcuColors"></LineCharts>
        </div>
      </div>
      <!--ROW 4-->
      <div class="row mt-5">
        <div class="col-xs-12 col-sm-12 col-md-12" v-if="arrVentilators.length > 0">
          <h2>On Ventilators</h2>
          <LineCharts :chartData="arrVentilators" :options="chartOptions" label="On Ventilators" :chartColors="onVentilatorsColors"></LineCharts>
        </div>
      </div>
      <!--ROW 5-->
      <div class="row mt-5">
        <div class="col-xs-12 col-sm-12 col-md-12" v-if="arrRecovered.length > 0">
          <h2>Recovered</h2>
          <LineCharts :chartData="arrRecovered" :options="chartOptions" label="Recovered" :chartColors="recoveredColors"></LineCharts>
        </div>
      </div>
      <!--ROW 6-->
      <div class="row mt-5">
        <div class="col-xs-12 col-sm-12 col-md-12" v-if="arrDeaths.length > 0">
          <h2>Deaths</h2>
          <LineCharts :chartData="arrDeaths" :options="chartOptions" label="Deaths" :chartColors="deathColors"></LineCharts>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import LineCharts from "@/components/LineCharts";
export default {
  name: 'App',
  components: {
    LineCharts
  },
  data () {
    return {
      arrPositive: [],
      arrHospitalized: [],
      arrInIcu: [],
      arrVentilators: [],
      arrRecovered: [],
      arrDeaths: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      },
      positiveChartColors: {
        borderColor: "#51ce54",
        pointBorderColor: "#51ce54",
        pointBackgroundColor: "rgba(63,238,49,0.61)",
        backgroundColor: "rgba(63,238,49,0.61)"
      },
      hospitalizedChartColors: {
        borderColor: "#005eff",
        pointBorderColor: "#005eff",
        pointBackgroundColor: "rgba(75,178,236,0.64)",
        backgroundColor: "rgba(75,178,236,0.64)"
      },
      inIcuColors: {
        borderColor: "#ff6700",
        pointBorderColor: "#ff6700",
        pointBackgroundColor: "rgba(236,157,80,0.71)",
        backgroundColor: "rgba(236,157,80,0.71)"
      },
      onVentilatorsColors: {
        borderColor: "#c002f8",
        pointBorderColor: "#c002f8",
        pointBackgroundColor: "rgba(230,187,237,0.84)",
        backgroundColor: "rgba(230,187,237,0.84)"
      },
      recoveredColors: {
        borderColor: "#f8e402",
        pointBorderColor: "#f8e402",
        pointBackgroundColor: "rgba(238,237,108,0.68)",
        backgroundColor: "rgba(238,237,108,0.68)"
      },
      deathColors: {
        borderColor: "#e50b35",
        pointBorderColor: "#e50b35",
        pointBackgroundColor: "rgba(239,73,97,0.76)",
        backgroundColor: "rgba(239,73,97,0.76)"
      }
    }
  },
  async created() {
    const { data } = await axios.get('https://covidtracking.com/api/us/daily')
    data.forEach(d => {
      const date = moment(d.date, 'YYYYMMDD').format('MM/DD')
      const {
        positive,
        hospitalizedCurrently,
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death
      } = d
      this.arrPositive.push({date, total: positive})
      this.arrHospitalized.push({date, total: hospitalizedCurrently})
      this.arrInIcu.push({date, total: inIcuCurrently})
      this.arrVentilators.push({date, total: onVentilatorCurrently})
      this.arrRecovered.push({date, total: recovered})
      this.arrDeaths.push({date, total: death})
    })
  }
}
</script>

<style>
</style>
