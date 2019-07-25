<template>
  <el-row class="content">
    <div id="myChart"
         :style="{width: '1000px', height: '500px'}"></div>
    <el-table ref="multipleTable"
              :data="list"
              tooltip-effect="dark"
              style="width: 100%">
      <el-table-column type="selection"
                       width="55">
      </el-table-column>
      <el-table-column prop="appid"
                       label="AppID"
                       width="120">
      </el-table-column>
      <el-table-column prop="statAt"
                       label="日期"
                       width="120">
      </el-table-column>
      <el-table-column prop="http_install"
                       label="HTTP安装"
                       width="120">
      </el-table-column>
      <el-table-column prop="udp_install"
                       label="UDP(安装)"
                       width="120">
      </el-table-column>
      <el-table-column prop="http_uninstall"
                       label="HTTP(卸载)"
                       width="120">
      </el-table-column>
      <el-table-column prop="udp_uninstall"
                       label="UDP(卸载)"
                       width="120">
      </el-table-column>
      <el-table-column prop="activation"
                       label="Activation"
                       width="120">
      </el-table-column>
      <el-table-column prop="alone_activation"
                       label="Alone_activation"
                       width="160">
      </el-table-column>
      <el-table-column prop="alone_ip"
                       label="Alone_ip"
                       width="120">
      </el-table-column>
      <el-table-column prop="highest_online"
                       label="Highest_online"
                       width="150">
      </el-table-column>
      <el-table-column prop="back_user"
                       label="Back_user"
                       width="120">
      </el-table-column>
      <el-table-column prop="uninstall_percent"
                       label="Uninstall_percent"
                       width="200">
      </el-table-column>
      <el-table-column prop="alt_day_retain_percent"
                       label="Alt_day_retain_percent"
                       width="200">
      </el-table-column>
      <el-table-column prop="week_retain_percent"
                       label="Week_retain_percent"
                       width="200">
      </el-table-column>
      <el-table-column prop="month_retain_percent"
                       label="Month_retain_percent"
                       width="200">
      </el-table-column>
    </el-table>
  </el-row>
</template>

<script>
import rp from 'request-promise-native'
import request from 'request'

export default {
  created () {
    this.post3()
  },
  data () {
    return {
      list: [],
      activation: [],
      activation_percent: [],
      alone_activation: [],
      alone_ip: [],
      alone_user: [],
      alt_day_retain_percent: [],
      appid: [],
      back_user: [],
      highest_online: [],
      http_install: [],
      http_uninstall: [],
      month_retain_percent: [],
      statAt: [],
      udp_install: [],
      udp_uninstall: [],
      udp_uninstall_day: [],
      uninstall_day_percent: [],
      uninstall_percent: [],
      week_retain_percent: [],
      index: {},
      size: [
        '2019-02-25',
        '2019-03-04',
        '2019-03-18',
        '2019-03-26',
        '2019-04-16',
        '2019-04-26',
        '2019-05-04',
        '2019-05-06',
        '2019-05-08',
        '2019-05-09'
      ],
      count: 0,
      id: ''
    }
  },
  mounted () {
    this.drawLine()
  },
  computed: {
    Done () {
      let list = 0
      for (let i in this.index) {
        list.push(this.index[i])
      }
      this.size = list
    }
  },

  methods: {
    drawLine () {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById('myChart'))
      // 绘制图表
      myChart.setOption({
        title: {
          text: '',
          subtext: ''
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['UDP(安装)', 'HTTP安装']
        },
        toolbox: {
          show: true,
          feature: {
            dataZoom: {
              yAxisIndex: 'none'
            },
            dataView: { readOnly: false },
            magicType: { type: ['line', 'bar'] },
            restore: {},
            saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: this.statAt
        },
        yAxis: {
          type: 'value',
          axisLabel: {
            formatter: '{value}'
          }
        },
        series: [
          {
            name: 'UDP(安装)',
            type: 'line',
            data: this.udp_install,
            markPoint: {
              data: [
                { type: 'max', name: '最大值' },
                { type: 'min', name: '最小值' }
              ]
            },
            markLine: {
              data: [{ type: 'average', name: '平均值' }]
            }
          },
          {
            name: 'HTTP安装',
            type: 'line',
            data: this.appid,
            markPoint: {
              data: [{ name: '周最低', value: 2, xAxis: 1, yAxis: 1.5 }]
            },
            markLine: {
              data: [
                { type: 'average', name: '平均值' },
                [
                  {
                    symbol: 'none',
                    x: '90%',
                    yAxis: 'max'
                  },
                  {
                    symbol: 'circle',
                    label: {
                      normal: {
                        position: 'start',
                        formatter: '最大值'
                      }
                    },
                    type: 'max',
                    name: '最高点'
                  }
                ]
              ]
            }
          }
        ]
      })
    },
    async post1 () {
      await this.$http
        .post('', {
          appid: 22,
          page: 0,
          pagesize: 10
        })
        .then(
          function (res) {
            console.log(typeof res)
            console.log(res)
            this.index = res
            console.log(typeof this.index)
            console.log(this.index)
          },
          function (res) {
            console.log(res.status)
          }
        )
    },
    async post2 () {
      await request.post(
        {
          url: '',
          form: {
            appid: 22,
            page: 0,
            pagesize: 10
          }
        },
        (httpResponse, body) => {
          console.log(typeof body)
          this.index = body
        }
      )
    },
    async post3 (ctx, next) {
      await this.MainPageModel(result => {
        if (result !== null) {
          console.log(typeof result)
          this.index = result.result
          this.com()
        } else {
          ctx.response.body = {
            success: false,
            data: '获取数据出错'
          }
        }
      })
    },
    com () {
      for (let i in this.index) {
        this.list.push(this.index[i])
      }
      for (let i in this.list) {
        this.activation.push(this.list[i].activation)
        this.activation_percent.push(this.list[i].activation_percent)
        this.alone_activation.push(this.list[i].alone_activation)
        this.alone_ip.push(this.list[i].alone_ip)
        this.alone_user.push(this.list[i].alone_user)
        this.alt_day_retain_percent.push(this.list[i].alt_day_retain_percent)
        this.appid.push(this.list[i].appid)
        this.back_user.push(this.list[i].back_user)
        this.highest_online.push(this.list[i].highest_online)
        this.http_install.push(this.list[i].http_install)
        this.http_uninstall.push(this.list[i].http_uninstall)
        this.month_retain_percent.push(this.list[i].month_retain_percent)
        this.statAt.push(this.list[i].statAt)
        this.udp_install.push(this.list[i].udp_install)
        this.udp_uninstall.push(this.list[i].udp_uninstall)
        this.udp_uninstall_day.push(this.list[i].udp_uninstall_day)
        this.uninstall_day_percent.push(this.list[i].uninstall_day_percent)
        this.uninstall_percent.push(this.list[i].uninstall_percent)
        this.week_retain_percent.push(this.list[i].week_retain_percent)
      }
      this.drawLine()
      console.log(Array.isArray(this.size))
      console.log(this.size)
      console.log(Array.isArray(this.statAt))
      console.log(this.statAt)
      console.log(this.uninstall_day_percent)
      console.log(this.list)
      console.log(this.index)
    },
    async MainPageModel (callback) {
      const sendData = {
        appid: 22,
        page: 0,
        pagesize: 10
      }
      await this.PostDataToApi(
        'http://api.mgr.xzdesktop.cqttech.com/api/MainPageList',
        sendData
      ).then(
        function (data) {
          callback(data)
        },
        () => {
          callback(null)
        }
      )
    },
    PostDataToApi (apiURL, sendData) {
      return new Promise(function (resolve, reject) {
        let options = {
          uri: apiURL,
          method: 'POST',
          json: true,
          encoding: null,
          headers: {
            'content-type': 'application/json'
          },
          body: sendData
        }
        rp(options)
          .then(function (body) {
            console.log(body)
            resolve(body)
          })
          .catch(function () {
            reject(new Error('wocuole'))
          })
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
.el-input {
  margin: 20px auto;
}

.todo-list {
  width: 100%;
  margin-top: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid #eee;
  overflow: hidden;
  text-align: left;

  .item {
    font-size: 20px;

    &.finished {
      text-decoration: line-through;
      color: #ddd;
    }
  }
}

.pull-right {
  float: right;
}
</style>