<template>
    <el-table
        :data="tableData"
        style="width: 100%;background-color: transparent;">
      <el-table-column
          prop="name"
          label="负责人">
      </el-table-column>
      <el-table-column
          prop="task_status"
          label="任务状态">
      </el-table-column>
      <el-table-column
          prop="create_time"
          label="日期"
          width="180"
          format = 'yyyy-MM-dd'
      >
      </el-table-column>
    </el-table>
</template>
<script>
export default {
  data () {
    return {
      // cdata: {
      // },
      tableData: []
    }
  },
  mounted () {
    this.$nextTick(function () {
      this.getTask()
    })
  },
  methods: {
    drawTimingFn () {
      this.setData();
      this.drawTiming = setInterval(() => {
        this.getAlarmRecent ()();
      }, 6000);
    },
    getTask () {
      this.tableLoading = true
      this.axios({
        method: 'get',
        url: '/monitors/statistics/recentTask'
      }).then(res => {
        this.tableData = res['data']['data']
      })
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
<style>
.el-table--enable-row-hover .el-table__body tr:hover > td {
  background-color: rgb(19, 75, 132);
}
</style>
