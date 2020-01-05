<template>
  <div>

    <el-main>
      <el-card class="box-card">
        <div slot="header" class="clearfix"><i class="el-icon-c-scale-to-original"></i> Simulators</div>

        <div class="simStatus" v-for="(simulator, index) in config.simulators">
          <el-image class="simImage" :src="url(simulator.frame)" />
          <span class="simName">
            {{ simulator.name }}
          </span>

          <el-button class="simDeleteButton" @click="deleteSim(simulator)" size="mini" type="danger" icon="el-icon-delete" circle></el-button>
          <el-button class="simRunningStatus" v-if="!simulator.running" type="danger" size="mini" icon="el-icon-close" circle></el-button>

        </div>



        <el-button @click="addSimulator()" size="medium" type="primary" round>Add Simulator</el-button>

      </el-card>
    </el-main>

  </div>
</template>

<script>
import cards from '../../cards.json'
import frames from '../../frames.json'
import defaultConfig from '../../defaultConfig.json'

  export default {
    name: 'SystemOverview',
    components: { },
    props: ['config'],
    data () {
      return {
        cards: cards,
        frames: frames,
        defaultConfig: defaultConfig,
        simulators: this.config.simulators
      }
    },
    methods: {
      url: function(f) {
        return './src/renderer/assets/frame-icon-' + f + '.png'
      },
      deleteSim: function (sim) {
        this.config.simulators = this.config.simulators.filter(item => item !== sim)
      },
      addSimulator: function() {
        var highestId = 0
        this.config.simulators.forEach(sim => {
          if (sim.id > highestId) {
            highestId = sim.id
          }
        })
        var newSim = JSON.parse(JSON.stringify(defaultConfig.simulators[0]))
        newSim.id = (parseInt(highestId) + 1).toString()
        this.config.simulators.push(newSim)
      }
    }
  }
</script>

<style>
.simStatus {
    margin-bottom: 1px;
    height: 40px;
  }
  .simImage {
    float: left;
    width: 100px;
    height: 40px;
    margin-right: 10px;
    margin-left: 10px;
  }
  .simName {
    float: left;
    height: 30px;
    padding-left: 10px;
    padding-top: 10px;
    font-weight: bold;
  }
  .simDeleteButton {
    float: right;
    margin-left: 50px;
  }
  .simRunningStatus {
    float: right;
  }
</style>
