<template>
  <div>
    <Chart :cdata="cdata" />
  </div>
</template>

<script>
import Chart from './chart.vue';
export default {
  data () {
    return {
      cdata: {
        xData: ["滚石", "塌方", "泥石流", "位移变形"],
        seriesData: [
          { value: 10, name: "滚石" },
          { value: 5, name: "塌方" },
          { value: 15, name: "泥石流" },
          { value: 25, name: "位移变形" }
        ]

      }
    }
  },
  components: {
    Chart,
  },
  mounted () {
    this.getTotals ();
  },
  methods: {
    getTotals () {
      this.axios({
        method: 'get',
        url: '/monitors/statistics/getTotals'
      }).then(res => {
        this.statisticRes = res['data']['data'];
        for(let item in this.statisticRes){
          this.statisticData[item] =  this.statisticRes[item] =='null'?0: parseFloat(this.statisticRes[item]);
          this.cdata.seriesData.value = this.statisticRes[item] =='null'?0: parseFloat(this.statisticRes[item]);
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
