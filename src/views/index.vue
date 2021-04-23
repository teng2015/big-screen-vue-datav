<template>
  <div id="index">
    <dv-full-screen-container class="bg">
      <dv-loading v-if="loading">Loading...</dv-loading>
      <div v-else class="host-body">
        <div class="d-flex jc-center">
          <dv-decoration-10 style="width:33.3%;height:.0625rem;" />
          <div class="d-flex jc-center">
            <dv-decoration-8 :color="['#568aea', '#000000']" style="width:2.5rem;height:.625rem;" />
            <div class="title">
              <span class="title-text">边坡变形智能预警平台</span>
              <dv-decoration-6
                class="title-bototm"
                :reverse="true"
                :color="['#50e3c2', '#67a1e5']"
                style="width:3.125rem;height:.1rem;"
              />
            </div>
            <dv-decoration-8
              :reverse="true"
              :color="['#568aea', '#000000']"
              style="width:2.5rem;height:.625rem;"
            />
          </div>
          <dv-decoration-10 style="width:33.3%;height:.0625rem; transform: rotateY(180deg);" />
        </div>

        <!-- 第二行 -->
        <div class="d-flex jc-between px-2">
          <div class="d-flex" style="width: 40%">
            <div
              class="react-right ml-4"
              style="width: 6.25rem; text-align: left;background-color: #0f1325;"
            >
              <span class="react-before"></span>
              <span class="text">廊坊 晴转多云  5℃/ 19℃</span>
            </div>
            <div class="react-right ml-3" style="background-color: #0f1325;">
              <a href="http://localhost:8082/6-1" target="_blank">
                <span class="text colorBlue">数据分析</span>
              </a>
            </div>
          </div>
          <div style="width: 40%" class="d-flex">
            <div class="react-left bg-color-blue mr-3">
              <a href="http://localhost:8082" target="_blank" style="color: white">
                <span class="text fw-b">
                  特路科技有限公司
                </span>
              </a>
            </div>
            <div
              class="react-left mr-4"
              style="width: 6.25rem; background-color: #0f1325; text-align: right;"
            >
              <span class="react-after"></span>
              <span class="text">{{dateYear}} {{dateWeek}} {{dateDay}}</span>
            </div>
          </div>
        </div>

        <div class="body-box">
          <!-- 第三行数据 -->
          <div class="content-box">
            <div>
              <dv-border-box-12>
                <centerLeft1 />
              </dv-border-box-12>
            </div>
            <div id="ctl2">
              <dv-border-box-12>
                <br>
                <div class="bg-color-black" style="width: 92%;margin-left: 4%">
                  <div class="d-flex pt-2 pl-2">
                    <span style="color:#5cd9e8">
                      <icon name="chart-pie"></icon>
                    </span>
                    <div class="d-flex">
                      <span class="fs-xl text mx-2">特路（北京）科技有限公司</span>
                      <dv-decoration-1 style="width:1.25rem;height:.25rem; position:relative;top:-.0375rem;" />
                    </div>
                  </div>
                  <span style="color:#5cd9e8">
                    <br><br>
                    <p style="width: 92%;margin-left: 4%;font-size: larger">特路（北京）科技有限公司成立于2015年初，国家、中关村高新技术企业，专注于自动驾驶汽车试验场和智慧交通，公司团队主要由汽车试验场、互联网软件、系统集成、人工智能四个领域的资深从业者组成，主要为智能网联汽车试验场提供规划设计、测试管控软件开发和测试技术研究。</p>
                  </span>
                  <br>
                  <centerLeft2 />
                </div>
              </dv-border-box-12>
            </div>
            <!-- 中间 -->
            <div>
              <center />
            </div>
            <!-- 中间 -->
            <div>
              <centerRight2 />
            </div>
            <div>
              <dv-border-box-13>
                <centerRight1 />
              </dv-border-box-13>
            </div>
          </div>

          <!-- 第四行数据 -->
          <div class="bototm-box">
            <dv-border-box-13>
              <bottomLeft />
            </dv-border-box-13>
            <dv-border-box-12>
              <bottomRight />
            </dv-border-box-12>
          </div>
        </div>
      </div>
    </dv-full-screen-container>
  </div>
</template>

<script>
import { formatTime } from '../utils/index.js'
import centerLeft1 from "./centerLeft1";
import centerLeft2 from "./centerLeft2";
import centerRight1 from "./centerRight1";
import centerRight2 from "./centerRight2";
import center from "./center";
import bottomLeft from "./bottomLeft";
import bottomRight from "./bottomRight";
export default {
  data () {
    return {
      loading: true,
      dateDay: null,
      dateYear: null,
      dateWeek: null,
      weekday: ["周日", "周一", "周二", "周三", "周四", "周五", "周六"],
    };
  },
  components: {
    centerLeft1,
    centerLeft2,
    centerRight1,
    centerRight2,
    center,
    bottomLeft,
    bottomRight
  },
  mounted () {
    this.timeFn();
    this.cancelLoading();
  },
  methods: {
    timeFn () {
      setInterval(() => {
        this.dateDay = formatTime(new Date(), 'HH: mm: ss');
        this.dateYear = formatTime(new Date(), 'yyyy-MM-dd');
        this.dateWeek = this.weekday[new Date().getDay()];
      }, 1000)
    },
    cancelLoading () {
      setTimeout(() => {
        this.loading = false;
      }, 500);
    },

  }
};
</script>

<style lang="scss">
@import '../assets/scss/index.scss';
#ctl2{
  //padding: 0.2rem;
  //height: 5.125rem;
  //min-width: 2rem;
  border-radius: 0.0625rem;
  .bg-color-black {
    height: 4.8125rem;
    border-radius: 0.125rem;
  }
}
</style>
