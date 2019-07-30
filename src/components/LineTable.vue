<!--<template>
  <div class="lineTable">
    <v-chart class="my-chart1" :options="zzper"/>
    <v-chart class="my-chart2" :options="bar"/>
  </div>
</template>

<script>
import ECharts from 'vue-echarts/components/ECharts'
import 'echarts/lib/chart/line'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/legend'
import 'echarts/lib/component/axis'
import 'echarts/lib/component/markLine'
import 'echarts/lib/component/markPoint'
import 'echarts/lib/component/title'

export default {
  name: 'lineTable',
  components: {
    'v-chart': ECharts
  },
  created () {
    this.GetcolConfigs()
    this.post()
  },
  data () {
    return {
      colConfigs: [],
      zzper: {},
      bar: {},
      index: {},
      list: [],
      site: [],
      statAt: [],
      AppID: [],
      numName: [],
      perName: [],
      num: [],
      per: [],
      numseries: [],
      numyAxis: [],
      perseries: [],
      peryAxis: []
    }
  },
  methods: {
    post () {
      this.$http.post('http://api.mgr.xzdesktop.cqttech.com/api/MainPageList', {
        appid: 22,
        page: 0,
        pagesize: 10
      }).then((res) => {
        console.log(res)
        if (res.status === 200) {
          for (let i in res.data.result) {
            this.list.push(res.data.result[i])
          }
          this.GetSite()
          this.CreatDrawLineo()
          this.CreatDrawLineu()
        }
      }, function (res) {
        console.log(res.status)
      })
    },
    GetcolConfigs () {
      this.colConfigs = [
      { prop: 'statAt', label: '日期', title: '杂' },
      { prop: 'appid', label: 'AppID', title: '杂' },
      { prop: 'http_install', label: 'HTTP安装量'  },
      { prop: 'udp_install', label: 'UDP安装量'  },
      { prop: 'http_uninstall', label: 'HTTP卸载量'  },
      { prop: 'udp_uninstall', label: 'UDP卸载量'  },
      { prop: 'activation', label: '总激活量'  },
      { prop: 'alone_activation', label: '独立激活量'  },
      { prop: 'alone_ip', label: '独立ip量'  },
      { prop: 'alone_user', label: '独立用户量'  },
      { prop: 'udp_uninstall_day', label: 'UDP当天卸载量'  },
      { prop: 'highest_online', label: '最高在线量'  },
      { prop: 'uninstall_day_percent', label: '当天卸载率'  },
      { prop: 'activation_percent', label: '激活率'  },
      { prop: 'back_user', label: '用户回头率'  },
      { prop: 'uninstall_percent', label: '卸载率'  },
      { prop: 'alt_day_retain_percent', label: '日留存率'  },
      { prop: 'week_retain_percent', label: '周留存率'  },
      { prop: 'month_retain_percent', label: '月留存率'  }
      ]
    },
    GetSite () {
      // 配置 site 里面包括折线图所需要的所有数据
      if (this.list == null) { return }
      for (let i in this.colConfigs) {
        let temp = []
        for (let j in this.list) {
          if (this.colConfigs[i].prop.toString() === 'statAt') {
            this.statAt.push(this.list[j][this.colConfigs[i].prop.toString()])
          } else if (this.colConfigs[i].prop.toString() === 'appid') {
            this.AppID.push(this.list[j][this.colConfigs[i].prop.toString()])
          } else {
            temp.push(this.list[j][this.colConfigs[i].prop.toString()])
          }
        }
        console.log(this.perName)
        if (this.colConfigs[i].title.toString() === '量') {
          this.numName.push(this.colConfigs[i].label)
          this.num.push(temp)
        }
        if (this.colConfigs[i].title.toString() === '率') {
          this.perName.push(this.colConfigs[i].label)
          this.per.push(temp)
        }
        temp = []
      }
      console.log(this.numName)
      console.log(this.statAt)
      console.log(this.num)
    },
    CreatDrawLineo () {
      // 绘制图表
      this.numseries = []
      this.numyAxis = []
      for (let i = 0; i < this.num.length; i++) {
        this.numseries.push({
          name: this.numName[i],
          type: 'line',
          data: this.num[i],
          markPoint: {data: [{ name: '周最低', value: 2, xAxis: 1, yAxis: 1.5 }]},
          markLine: {data: [{ type: 'average', name: '平均值' }, [{symbol: 'none', x: '90%', yAxis: 'max'},
          {symbol: 'circle', label: {normal: {position: 'start', formatter: '最大值'}}, type: 'max', name: '最高点'}]]}
        })
      }
      this.zzper = {
        title: {
          text: '统计信息'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: this.numName
        },
        xAxis: {
          type: 'category',
          boundaryGap: true,
          data: this.statAt
        },
        yAxis: {
          type: 'value'
        },
        series: this.numseries
      }
    },
    CreatDrawLineu () {
      // 绘制图表
      this.perseries = []
      this.peryAxis = []
      for (let i = 0; i < this.per.length; i++) {
        this.perseries.push({
          name: this.perName[i],
          type: 'line',
          data: this.per[i],
          yAxisIndex: i,
          areaStyle: { opacity: 0.3 },
          markPoint: {data: [{ type: 'max', name: '最大值' }, { type: 'min', name: '最小值' }
          ]},
          markLine: {data: [{ type: 'average', name: '平均值' }]}
        })
        this.peryAxis.push({
          type: 'value'
        })
      }
      this.bar = {
        title: {
          text: '统计信息',
          subtext: ''
        },
        tooltip: {
          trigger: 'axis'
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
        legend: {
          data: this.perName
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: this.statAt
        },
        yAxis: this.peryAxis,
        series: this.perseries
      }
    }
  }
}
</script>

<style scoped>
.my-chart1 {
  position:fixed;
	top:50px;
	right:0px;
  width: 1200px;
  height: 500px;
}
.my-chart2 {
  position:fixed;
	top:-450px;
	left:1200px;
  width: 1200px;
  height: 500px;
}
</style> -->
 

 <template>
  <div class="lineTable">
    <v-chart class="my-chart3" :options="liner"/>
    <v-chart class="my-chart1" :options="zzper"/>
    <v-chart class="my-chart2" :options="bar"/>
  </div>
</template>

<script>
import ECharts from 'vue-echarts/components/ECharts'
import 'echarts/lib/chart/line'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/legend'
import 'echarts/lib/component/axis'
import 'echarts/lib/component/markLine'
import 'echarts/lib/component/markPoint'
import 'echarts/lib/component/title'

export default {
  name: 'lineTable',
  components: {
    'v-chart': ECharts
  },
  created () {
    this.post()
  },
  data () {
    return {
      colConfigs: [],
      zzper: {},
      bar: {},
      liner: {},
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
      week_retain_percent: []
    }
  },
  methods: {
    post () {
      this.$http.post('', {
        appid: 22,
        page: 0,
        pagesize: 10
      }).then((res) => {
        console.log(res)
        if (res.status === 200) {
          for (let i in res.data.result) {
            this.list.push(res.data.result[i])
          }
          this.GetSite()
          this.CreatDrawLineo()
          this.CreatDrawLineu()
          this.CreatDrawLiner()
        }
      }, function (res) {
        console.log(res.status)
      })
    },
    GetSite () {
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
    },
    CreatDrawLineo () {
      // 绘制图表
      this.zzper = {
        tooltip: {
          trigger: 'axis'
        },
        xAxis: {
          type: 'category',
          boundaryGap: true,
          data: this.statAt
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: 'HTTP安装量',
            type: 'line',
            data: this.http_install
          }, {
            name: 'UDP安装量',
            type: 'line',
            data: this.udp_uninstall
          }, {
            name: '总激活量',
            type: 'line',
            data: this.activation
          }
        ]
      }
    },
    CreatDrawLiner () {
      // 绘制图表
      this.liner = {
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['UDP卸载量', 'HTTP卸载量', 'UDP当天卸载量']
        },
        xAxis: {
          type: 'category',
          boundaryGap: true,
          data: this.statAt
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: 'HTTP卸载量',
            type: 'line',
            data: this.http_uninstall
          }, {
            name: 'UDP卸载量',
            type: 'line',
            data: this.udp_uninstall
          }, {
            name: 'UDP当天卸载量',
            type: 'line',
            data: this.udp_uninstall_day
          }
        ]
      }
    },
    CreatDrawLineu () {
      // 绘制图表
      this.bar = {
        tooltip: {
          trigger: 'axis'
        },
        xAxis: {
          type: 'category',
          boundaryGap: true,
          data: this.statAt
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: '日留存率',
            type: 'line',
            data: this.alt_day_retain_percent
          }, {
            name: '卸载率',
            type: 'line',
            data: this.uninstall_percent
          }, {
            name: '激活率',
            type: 'line',
            data: this.activation_percent
          }
        ]
      }
    }
  }
}
</script>

<style scoped>
.my-chart1 {
  position:fixed;
	top:-80px;
	right:-100px;
  width: 1200px;
  height: 500px;
}
.my-chart2 {
  position:fixed;
	top:-580px;
	left:1150px;
  width: 1200px;
  height: 500px;
}
.my-chart3 {
  position:fixed;
	top:0px;
	right:30px;
  width: 2500px;
  height: 300px;
}
</style>
 