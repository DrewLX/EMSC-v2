<template>
<div class="cardSelector">
  <div class="selectCard" v-on:click="selectCard">
    <el-image class="selectCardImage" :src="'./src/renderer/assets/' + cards[card].file90" />
    <div class="selectCardLabel" v-bind:class="cards[card].topLabel">{{ cards[card].topLabel }}</div>
    <div class="selectCardText">
        <strong>{{ cards[card].name }}</strong>
        <br />
        <i>{{ cards[card].desc }}</i>
      </div>
  </div>
</div>
</template>

<script>
import cards from '../../cards.json'

export default {
  name: 'CardSelect',
  props: ['card'],
  components: { },
  data () {
    return {
      cards: cards
    }
  },
  methods: {
    selectCard: function (event) {
      this.$emit('selectcard')
    }
  },
  computed: {
  }
}
</script>

<style>
  .selectCardImage {
    background-color: red;
    width: 182px;
    height: 45px;
    float: left;
  }
  .selectCardText {
    float: left;
    width: calc(100% - 250px);
    height: 45px;
    background-color: #2c3539;
    border-left: 1px solid white;
  }
  .selectCardLabel {
    width: 47px;
    height: 45px;
    float: left;
    vertical-align: middle;
    font-weight: bold;
    border-left: 1px solid white;
  }
  .selectCard {
    .background-color: blue;
    color: white;
    width: 100%;
    height: 47px;
    margin-bottom: 2px;
  }
</style>
