<template>
  <div id="centreLeft1">
    <div class="bg-color-black">
      <div class="d-flex pt-2 pl-2">
        <span style="color:#5cd9e8">
          <icon name="chart-bar"></icon>
        </span>
        <div class="d-flex">
          <span class="fs-xl text mx-2">报警分布统计</span>
          <dv-decoration-3 style="width:1.25rem;height:.25rem; position:relative;top:-.0375rem;" />
        </div>
      </div>
      <div class="d-flex jc-center">
        <CentreLeft1Chart />
      </div>
      <!-- 4个主要的数据 -->
      <div class="bottom-data">
        <div class="item-box" >
          <div class="d-flex">
            <span class="coin"></span>
<!--            <dv-digital-flop :end-val="statisticData.task_total" style="width:2.5rem;height:.625rem;" />-->
          </div>
          <p class="text" style="text-align: center;">
              塌方：<count-to :start-val="0" :end-val="statisticData.collapse_total" />
            <span class="colorYellow">(次)</span>
          </p>
        </div>
        <div class="item-box" >
          <div class="d-flex">
            <span class="coin"></span>
<!--            <dv-digital-flop :end-val="statisticData.stone_total" style="width:2.5rem;height:.625rem;" />-->
          </div>
          <p class="text" style="text-align: center;">
            滚石：<count-to :start-val="0" :end-val="statisticData.stone_total" />
            <span class="colorYellow">(次)</span>
          </p>
        </div>
        <div class="item-box" >
          <div class="d-flex">
            <span class="coin"></span>
<!--            <dv-digital-flop :end-val="statisticData.flow_total" style="width:2.5rem;height:.625rem;" />-->
          </div>
          <p class="text" style="text-align: center;">
            泥石流：<count-to :start-val="0" :end-val="statisticData.flow_total" />
            <span class="colorYellow">(次)</span>
          </p>
        </div>
        <div class="item-box" >
          <div class="d-flex">
            <span class="coin"></span>
<!--            <dv-digital-flop :end-val="statisticData.move_total" style="width:2.5rem;height:.625rem;" />-->
          </div>
          <p class="text" style="text-align: center;">
            位移变形：<count-to :start-val="0" :end-val="statisticData.move_total" />
            <span class="colorYellow">(次)</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import countTo from 'vue-count-to';
import CentreLeft1Chart from "@/components/echart/centerLeft/centerLeft1Chart";
export default {
  components: {
    countTo,
    CentreLeft1Chart
  },
  data() {
    return {
      config: {
        lineWidth: 30,
        activeRadius: "80%",
        radius: "75%",
        activeTimeGap: 2000,
        // data: [
        //   {
        //     name: "周口",
        //     value: 55
        //   },
        //   {
        //     name: "南阳",
        //     value: 120
        //   },
        //   {
        //     name: "西峡",
        //     value: 78
        //   },
        //   {
        //     name: "驻马店",
        //     value: 66
        //   },
        //   {
        //     name: "新乡",
        //     value: 80
        //   }
        // ]
      },
      statisticData:{
        move_total:0,
        flow_total:0,
        stone_total:0,
        collapse_total:0
      }
    };
  },
  mounted() {
    this.changeTiming();
  },
  methods: {
    changeTiming() {
      setInterval(() => {
        // this.changeNumber();
        this.getTotals();
      }, 3000);
    },
    getTotals () {
      this.axios({
        method: 'get',
        url: '/monitors/statistics/getTotals'
      }).then(res => {
        this.statisticRes = res['data']['data'];
        for(let item in this.statisticRes){
          this.statisticData[item] =  this.statisticRes[item] =='null'?0: parseFloat(this.statisticRes[item]);
        }
        this.statisticalLoading = false;
      })
    }
  }
};
</script>

<style lang="scss">
#centreLeft1 {
  padding: 0.2rem;
  height: 5.125rem;
  min-width: 3.75rem;
  border-radius: 0.0625rem;
  .bg-color-black {
    height: 4.8125rem;
    border-radius: 0.125rem;
  }
  .text {
    color: #c3cbde;
  }
  .chart-box {
    margin-top: 0.2rem;
    width: 2.125rem;
    height: 2.125rem;
    .active-ring-name {
      padding-top: 0.125rem;
    }
  }

  .bottom-data {
    .item-box {
      float: right;
      position: relative;
      width: 50%;
      color: #d3d6dd;
      // 金币
      .coin {
        position: absolute;
        left: 0.1rem;
        top: 0.2125rem;
        font-size: 0.25rem;
        color: #ffc107;
      }
      .colorYellow {
        color: yellowgreen;
      }
    }
  }
}
</style>
