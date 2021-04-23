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
      statisticData: {
        total: 0
      },
      statisticRes: {},
      cdata: {
        category: [],
        lineData: [],
        barData: [],
        rateData: [],
      }
    };
  },
  components: {
    Chart,
  },
  mounted () {
    this.setData();

  },
  methods: {
    // 根据自己的业务情况修改
    async setData () {
      // 先获取绘制图表的必要数据
      let res = await this.getStatistic();
      this.statisticRes = res['data']['data'];
      this.statisticData['total'] =  this.statisticRes['total'] =='null'?0: parseFloat(this.statisticRes['total']);
      // 序列每个图表需要的数据
      for(let item in this.statisticRes){
        if(item !== 'total'){
          this.statisticRes[item].forEach(v => {
            // this.cdata.rateData.push(rate.toFixed(2));
            this.cdata.category.push(v['x_axis']);
            this.cdata.lineData.push(v['y_axis']);
            this.cdata.barData.push(v['y_axis2']);
          })
        }
      }
      for (let i = 0; i < this.cdata.barData.length -1; i++) {
        let rate = this.cdata.barData[i] / this.cdata.lineData[i];
        this.cdata.rateData.push(rate.toFixed(2));
      }
    },
    getStatistic(){
      return this.axios({
        method: 'GET',
        url: '/monitors/statistics/point'
      })
    }
  }
};
</script>

<style lang="scss" scoped>
</style>
