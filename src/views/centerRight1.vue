<template>
  <div id="centreRight1">
    <div class="bg-color-black">
    <el-table
        :data="tableData"
        style="width: 100%;background-color: transparent;">
      <el-table-column
          prop="status"
          label="状态">
      </el-table-column>
      <el-table-column
          prop="laebl"
          label="类型">
      </el-table-column>
      <el-table-column
          prop="subTime"
          label="日期"
          width="180">
      </el-table-column>
    </el-table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: []
    };
  },
  components: {},
  mounted () {
    this.$nextTick(function () {
      this.getAlarmRecent()
    })
  },
  methods: {
    drawTimingFn () {
      this.setData();
      this.drawTiming = setInterval(() => {
        this.getAlarmRecent ()();
      }, 6000);
    },
    getAlarmRecent () {
      this.axios({
        method: 'get',
        url: '/monitors/alarm/getAlarmRecent'
      }).then(res => {
        this.tableData = res['data']['data']
      })
    }
  }
};
</script>

<style lang="scss">
#centreRight1 {
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
  .body-box {
    border-radius: 0.125rem;
    overflow: hidden;
  }
}
</style>
<style>

.el-table {
  /* 表格字体颜色 */
  color: white;
  /* 表格边框颜色 */
  /* border: 0.5px solid #758a99; */
  height: 500px;
}

/* 表格内背景颜色 */
.el-table th,
.el-table tr,
.el-table td {
  border: 0;
  background-color: transparent;
}

/* 双数行背景颜色 */
.el-table--striped .el-table__body tr.el-table__row--striped td {

  background-color: #fff;
  background-color: rgba(148, 144, 144, 0.3)
}

/* 使表格背景透明 */
.el-table th,
.el-table tr {
  background-color: transparent;
}

/* 删除表格下横线 */
.el-table::before {
  left: 0;
  bottom: 0;
  width: 100%;
  height: 0px;
}

/* 表格表头字体颜色 */
.el-table thead {
  color: white;
  font-weight: 500;
  background-color: rgba(148, 144, 144, 0.3)
}

</style>
