<template>
  <div id="center">
    <div class="up">
      <div class="bg-color-black item">
        <p class="ml-3 colorBlue fw-b">累计任务量</p>
        <p class="text" style="text-align: center;">
          <count-to :start-val="0" :end-val="statisticData.task_total" style="width:1.25rem;height:.625rem;"/>
          <span class="colorYellow">(次)</span>
        </p>
      </div>
      <div class="bg-color-black item">
        <p class="ml-3 colorBlue fw-b">完成任务量</p>
        <p class="text" style="text-align: center;">
          <count-to :start-val="0" :end-val="statisticData.donetask_total" style="width:1.25rem;height:.625rem;"/>
          <span class="colorYellow">(次)</span>
        </p>
      </div>
      <div class="bg-color-black item">
        <p class="ml-3 colorBlue fw-b">报警数量</p>
        <p class="text" style="text-align: center;">
          <count-to :start-val="0" :end-val="statisticData.alarm_total" style="width:1.25rem;height:.625rem;"/>
          <span class="colorYellow">(次)</span>
        </p>
      </div>

      <div class="bg-color-black item">
        <p class="ml-3 colorBlue fw-b">预警数量</p>
        <p class="text" style="text-align: center;">
          <count-to :start-val="0" :end-val="statisticData.warn_total" style="width:1.25rem;height:.625rem;"/>
          <span class="colorYellow">(次)</span>
        </p>
      </div>

      <div class="bg-color-black item">
        <p class="ml-3 colorBlue fw-b">本月报警数量</p>
        <p class="text" style="text-align: center;">
          <count-to :start-val="0" :end-val="statisticData.alarm_month_total" style="width:1.25rem;height:.625rem;"/>
          <span class="colorYellow">(次)</span>
        </p>
      </div>
      <div class="bg-color-black item">
        <p class="ml-3 colorBlue fw-b">本月预警数量</p>
        <p class="text" style="text-align: center;">
          <count-to :start-val="0" :end-val="statisticData.warn_month_total" style="width:1.25rem;height:.625rem;"/>
          <span class="colorYellow">(次)</span>
        </p>
      </div>
    </div>

    <div class="down">
      <div class="ranking bg-color-black">
        <span style="color:#5cd9e8">
          <icon name="align-left"></icon>
        </span>
        <span class="fs-xl text mx-2 mb-1">负责人完成任务数量</span>
        <dv-scroll-ranking-board :config="ranking" style="height:2.75rem" />
      </div>
      <div class="percent">
        <div class="item bg-color-black">
          <span>预警占比</span>
          <CenterChart :id="rate[0].id" :tips="rate[0].tips" :colorObj="rate[0].colorData" />
        </div>
        <div class="item bg-color-black">
          <span>报警占比</span>
          <CenterChart :id="rate[1].id" :tips="rate[1].tips" :colorObj="rate[1].colorData" />
        </div>
        <div class="item bg-color-black">
          <span>预警占比</span>
          <CenterChart :id="rate[2].id" :tips="rate[2].tips" :colorObj="rate[2].colorData" />
        </div>
        <div class="item bg-color-black">
          <span>任务完成率</span>
          <CenterChart :id="rate[3].id" :tips="rate[3].tips" :colorObj="rate[3].colorData" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CenterChart from "@/components/echart/center/centerChartRate";
import countTo from 'vue-count-to';
export default {
  data () {
    return {
      statisticData:{
        task_total: 0,
        donetask_total: 0,
        warn_total: 0,
        alarm_total:0,
        alarm_month_total:0,
        warn_month_total:0
      },
      titleItem: [
        {
          title: "今年达标任务个数",
          number: {
            number: [100],
            toFixed: 1,
            content: "{nt}"
          }
        }
      ],
      ranking: {
        data: [
          // {
          //   name: "worker5",
          //   value: 80
          // }
        ],
        carousel: "single",
        unit: "次"
      },
      // 通过率和达标率的组件复用数据
      rate: [
        {
          id: "centerRate1",
          tips: 20,
          colorData: {
            textStyle: "#3fc0fb",
            series: {
              color: ["#00bcd44a", "transparent"],
              dataColor: {
                normal: "#03a9f4",
                shadowColor: "#97e2f5"
              }
            }
          }
        },
        {
          id: "centerRate2",
          tips: 80,
          colorData: {
            textStyle: "#67e0e3",
            series: {
              color: ["#faf3a378", "transparent"],
              dataColor: {
                normal: "#ff9800",
                shadowColor: "#fcebad"
              }
            }
          }
        },
        {
          id: "centerRate3",
          tips: 10,
          colorData: {
            textStyle: "#3fc0fb",
            series: {
              color: ["#00bcd44a", "transparent"],
              dataColor: {
                normal: "#03a9f4",
                shadowColor: "#97e2f5"
              }
            }
          }
        },
        {
          id: "centerRate4",
          tips: 30,
          colorData: {
            textStyle: "#67e0e3",
            series: {
              color: ["#faf3a378", "transparent"],
              dataColor: {
                normal: "#ff9800",
                shadowColor: "#fcebad"
              }
            }
          }
        }
      ]
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
      }, 10000);
    },
    getTotals () {
      this.axios({
        method: 'get',
        url: '/monitors/statistics/getTotals'
      }).then(res => {
        this.statisticRes = res['data']['data'];
        this.ranking = res['data']['data'];
        this.rate.tips = res['data']['data'];
        for(let item in this.statisticRes){
          this.statisticData[item] =  this.statisticRes[item] =='null'?0: parseFloat(this.statisticRes[item]);
        }
      })
    }
  },
  components: {
    countTo,
    CenterChart
  }
};
</script>

<style lang="scss" scoped>
#center {
  display: flex;
  flex-direction: column;
  .up {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    .item {
      border-radius: 0.0625rem;
      padding-top: 0.2rem;
      margin-top: 0.1rem;
      width: 32%;
      height: 0.875rem;
    }
  }
  .down {
    padding: 0.07rem 0.05rem;
    padding-bottom: 0;
    width: 100%;
    display: flex;
    height: 3.1875rem;
    justify-content: space-between;
    .bg-color-black {
      border-radius: 0.0625rem;
    }
    .ranking {
      padding: 0.125rem;
      width: 59%;
    }
    .percent {
      width: 40%;
      display: flex;
      flex-wrap: wrap;
      .item {
        width: 50%;
        height: 1.5rem;
        span {
          margin-top: 0.0875rem;
          display: flex;
          justify-content: center;
        }
      }
      .water {
        width: 100%;
      }
    }
  }
}
</style>
