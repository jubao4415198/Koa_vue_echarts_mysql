<template>
<div>
  <LineTable></LineTable>             
  <el-table class="todolist" id="customers" :data="list" >
  <template v-for="colConfig in colConfigs">
    <slot v-if="colConfig.slot" :name="colConfig.slot"/>
    <el-table-column v-bind="colConfig"></el-table-column>
  </template>  
  </el-table>
  <br/>
  <br/>
</div >
</template>


<script>
import LineTable from './LineTable'

export default {
  name: 'todolist',
  components: {
    LineTable
  },
  created () {
    this.GetcolConfigs()
    this.post1()
  },
  data () {
    return {
      colConfigs: [],
      zzper: {},
      bar: {},
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
    post1 () {
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
        }
      }, function (res) {
        console.log(res.status)
      })
    },
    GetcolConfigs () {
      this.colConfigs = [
      { prop: 'statAt', label: '日期', width: '120' },
      { prop: 'appid', label: 'AppID', width: '80' },
      { prop: 'http_install', label: 'HTTP安装量', width: '120' },
      { prop: 'udp_install', label: 'UDP安装量', width: '110' },
      { prop: 'http_uninstall', label: 'HTTP卸载量', width: '120' },
      { prop: 'udp_uninstall', label: 'UDP卸载量', width: '110' },
      { prop: 'activation', label: '总激活量', width: '100' },
      { prop: 'alone_activation', label: '独立激活量', width: '110' },
      { prop: 'alone_ip', label: '独立ip量', width: '100' },
      { prop: 'alone_user', label: '独立用户量', width: '110' },
      { prop: 'udp_uninstall_day', label: 'UDP当天卸载量', width: '140' },
      { prop: 'highest_online', label: '最高在线量', width: '110' },
      { prop: 'uninstall_day_percent', label: '当天卸载率', width: '110' },
      { prop: 'activation_percent', label: '激活率', width: '90' },
      { prop: 'back_user', label: '用户回头率', width: '110' },
      { prop: 'uninstall_percent', label: '卸载率', width: '90' },
      { prop: 'alt_day_retain_percent', label: '日留存率', width: '100' },
      { prop: 'week_retain_percent', label: '周留存率', width: '100' },
      { prop: 'month_retain_percent', label: '月留存率', width: '100' },
      { slot: 'opt' }
      ]
    }
  }
}
</script>

<style>
.todolist
{
	top:-600px;
  left: 200px;
  width: 2030;
}
th 
{
border:1px solid gray;
}
td 
{
border:1px solid gray;
padding:0px;
}
.el-table .cell, .el-table th>div
{
padding:0px;
}
.el-table .cell, .el-table th>div
{
padding:0px;
}
.el-table .cell, .el-table th>div{
  padding-left: 0px;
  padding-right: 0px;
}
</style>