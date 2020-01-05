<template>
<div>

<el-card class="box-card box-frame">
  <div slot="header" class="clearfix" v-on:click="editFrameVisible=true">
    <span class="frameName">{{ frames[simulator.frame].name }} : {{ simulator.name }}</span>
  </div>
  <div class="frame-bg" v-bind:class="frameStyle">
    <Card
    v-for="(card, index) in simulator.cards"
    v-if="frames[simulator.frame].slotVisibility[index]"
    v-bind:card="card"
    v-bind:index="index"
    v-on:editcard="editCard(index)"
    v-bind:cardsSideways="frames[simulator.frame].cardsSideways" />
  </div>
</el-card>

<el-main style="width: 740px; margin: auto;" v-if="this.frames[simulator.frame].expCardOptions.length > 0 || this.frames[simulator.frame].vpuCardOptions.length > 0">
  <el-card class="box-card">
    <div slot="header" class="clearfix"><i class="el-icon-setting"></i> Settings</div>
  <el-row :gutter="20">
    <el-col :span="12" v-if="this.frames[simulator.frame].expCardOptions.length > 0">
      Exp Link Cards:
      <el-radio-group v-model="expCount" size="medium">
        <el-radio-button v-for="option in frames[simulator.frame].expCardOptions" v-on:click="setExpCards(option)" v-bind:label="option"></el-radio-button>
      </el-radio-group>
    </el-col>

    <el-col :span="12" v-if="this.frames[simulator.frame].vpuCardOptions.length > 0">
      VPU Cards:
      <el-radio-group v-model="vpuCount" size="medium">
        <el-radio-button v-for="option in frames[simulator.frame].vpuCardOptions" v-on:click="setExpCards(option)" v-bind:label="option"></el-radio-button>
      </el-radio-group>
    </el-col>
  </el-row>
  </el-card>
</el-main>

<el-main style="width: 740px; margin: auto;">
  <el-card class="box-card">
    <div slot="header" class="clearfix"><i class="el-icon-link"></i> Links</div>
  Links!
  </el-card>
</el-main>

<el-dialog
  :visible.sync="editCardVisible"
  width="70%">
  <CardSelect v-for="opt in editingCardOptions" v-bind:card="opt" v-on:selectcard="selectCard(opt)"/>
</el-dialog>

<el-dialog
  title="Select Frame"
  :visible.sync="editFrameVisible"
  width="70%">
  <el-button v-for="(frame, index) in frames" v-on:click="selectFrame(index)" type="primary" icon="el-icon-tickets">{{ frame.name }}</el-button>
</el-dialog>
</div>
</template>

<script>
import Card from './Card'
import CardSelect from './CardSelect'
import cards from '../../cards.json'
import frames from '../../frames.json'

export default {
  name: 'Frame',
  components: {
    Card, CardSelect
  },
  props: ['simulator'],
  data () {
    return {
      cards: cards,
      frames: frames,
      editCardVisible: false,
      editingCardAtIndex: null,
      editingCardOptions: null,
      editFrameVisible: false,
      expCount: 0,
      vpuCount: 0
    }
  },
  computed: {
    frameStyle () {
      return 'frameStyle' + [this.simulator.frame]
    }
  },
  watch: {
    expCount: function() {
      var leftToSet = this.expCount
      var slots = this.frames[this.simulator.frame].slotCompatability
      Object.keys(slots).forEach(key => {
        if (slots[key].includes('70')) {
          if (leftToSet > 0) {
            this.simulator.cards[key] = '70'
            leftToSet = leftToSet - 1
          } else {
            if (this.simulator.cards[key] === '70') {
              this.simulator.cards[key] = 'x'
            }
          }
        }
      })
    },
    vpuCount: function() {
      var leftToSet = this.vpuCount
      var slots = this.frames[this.simulator.frame].slotCompatability
      Object.keys(slots).forEach(key => {
        if (slots[key].includes('50')) {
          if (leftToSet > 0) {
            this.simulator.cards[key] = '50'
            leftToSet = leftToSet - 1
          } else {
            if (this.simulator.cards[key] === '50') {
              this.simulator.cards[key] = 'x'
            }
          }
        }
      })
    }
  },
  methods: {
    editCard (index) {
      this.editingCardAtIndex = index
      this.editingCardOptions = this.frames[this.simulator.frame].slotCompatability[index]
      this.editCardVisible = true
    },
    selectCard (card) {
      console.log(this.editingCardAtIndex, card)
      this.simulator.cards[this.editingCardAtIndex] = card
      this.editCardVisible = false
      this.countExpCards()
      this.countVpuCards()
    },
    selectFrame (selectedFrame) {
      this.simulator.frame = selectedFrame
      this.simulator.cards = frames[selectedFrame].defaultCards
      this.editFrameVisible = false
      this.countExpCards()
      this.countVpuCards()
    },
    setExpCards (num) {
      console.log('setExpCards', num)
    },
    countExpCards () {
      var count = 0
      this.simulator.cards.forEach(card => {
        if (card === "70") {
          count = count + 1
        }
      })
      this.expCount = count
    },
    countVpuCards () {
      var count = 0
      this.simulator.cards.forEach(card => {
        if (card === "50") {
          count = count + 1
        }
      })
      this.vpuCount = count
    }
  },
  mounted: function () {
    this.countExpCards()
    this.countVpuCards()
  }
}
</script>

<style>
  .frame-bg {
    background-color: grey;
    padding-left: 2px;
    padding-top: 2px;
    margin: auto;
  }
  .frameStyle0 {
    background-image: url("../assets/frame-0.png");
    width: 662px;
    height: 250px;
  }
  .frameStyle1 {
    background-image: url("../assets/frame-1.png");
    width: 426px;
    height: 209px;
    padding-left: 236px;
  }
  .frameStyle2 {
    background-image: url("../assets/frame-2.png");
    width: 370px;
    height: 72px;
    padding-left: 290px;
  }
  .frameStyle3 {
    background-image: url("../assets/frame-3.png");
    width: 371px;
    height: 72px;
    padding-left: 226px;
  }
  .box-frame {
    background-color: #1C1E22;
    width: 700px;
    margin: auto;
  }
  .frameName {
    color: white;
    font-weight: bold;
  }
</style>
