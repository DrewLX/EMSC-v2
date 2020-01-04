<template>
<div id="app">

  <el-tabs v-model="activeName" @tab-click="handleClick">

    <el-tab-pane name="overview">
      <span slot="label"><i class="el-icon-setting"></i> Overview Settings</span>
      <SystemOverview />
    </el-tab-pane>

    <el-tab-pane name="console">
      <span slot="label"><i class="el-icon-tickets"></i> Console</span>
      System Console?
    </el-tab-pane>

    <el-tab-pane v-for="simulator in config.simulators" v-bind:name="simulator.id">
      <span slot="label"><i class="el-icon-c-scale-to-original"></i> {{ simulator.name }}</span>
      <Frame v-bind:simulator="simulator" />
    </el-tab-pane>

  </el-tabs>

</div>
</template>

<script>
import SystemOverview from '@/components/SystemOverview'
import Frame from '@/components/Frame'

import defaultConfig from '../defaultConfig.json'

export default {
  name: 'e2scv2',
  components: {
    SystemOverview, Frame
  },
  data () {
    return {
      activeName: '0',
      config: defaultConfig
    }
  },
  methods: {
    handleClick (tab, event) {
      console.log(tab, event)
    }
  }
}
</script>

<style>
#app {
  font-family: Helvetica, sans-serif;
  text-align: center;
}
</style>
