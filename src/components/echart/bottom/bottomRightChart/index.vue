<template>
  <div>
    <Chart :cdata="cdata" />
  </div>
</template>

<script>
import Chart from './chart.vue'
export default {
  data () {
    return {
      drawTiming: null,
      cdata: {
        year: null,
        weekCategory: [],
        radarData: [],
        radarDataAvg: [],
        maxData: 120,
        weekMaxData: [],
        weekLineData: []
      },
      statisticData: {
        total: 0
      },
      move: {
        xData: [],
        yData: [],
        yData2: [],
        yData3: [],
        yData4: []
      },
      total: 0,
    }
  },
  components: {
    Chart,
  },
  mounted () {
    this.drawTimingFn();
  },
  beforeDestroy () {
    clearInterval(this.drawTiming);
  },
  methods: {
    drawTimingFn () {
      this.setData();
      this.drawTiming = setInterval(() => {
        this.setData();
      }, 6000);
    },
    getStatistic(){
      return this.axios({
        method: 'GET',
        url: '/monitors/statistics/movelist'
      }).then(res =>{
        this.tableData = res['data']['data'];
        this.total = res['data']['total'];
      })
    },
    async setData () {
      // 清空轮询数据
      this.cdata.weekCategory = [];
      this.cdata.weekMaxData = [];
      this.cdata.weekLineData = [];
      this.cdata.radarData = [];
      this.cdata.radarDataAvg = [];
      let dateBase = new Date();
      this.cdata.year = dateBase.getFullYear();
      // 周数据
      for (let i = 0; i < 7; i++) {
        // 日期
        // let date = new Date();
        // this.cdata.weekCategory.unshift([date.getMonth() + 1, date.getDate()-i].join("/"));
        let x = '机位1点位1';
        this.cdata.weekCategory.push(x);
        // 折线图数据
        this.cdata.weekMaxData.push(this.cdata.maxData);
        let distance = Math.round(Math.random() * 100 + 5);
        this.cdata.weekLineData.push(distance);
      }

    }
  }
};
</script>

<style lang="scss" scoped>
</style>
