<template>
<div class="card" v-bind:class="{rotated : cardsSideways}">
  <p class="topLabel" v-bind:class="cards[card].topLabel">{{ cards[card].topLabel }}</p>
  <el-image
      class="cardImage"
      :src="this.url"
      v-on:click="editCard"
      v-bind:class="{cardsSideways : cardsSideways}"
      />
</div>
</template>

<script>
import cards from '../../cards.json'

export default {
  name: 'Card',
  props: ['card', 'index', 'cardsSideways'],
  components: {},
  data () {
    return {
      cards: cards
    }
  },
  methods: {
    editCard: function (event) {
      this.$emit('editcard')
      // alert('Edit Card ' + this.index)
    }
  },
  computed: {
    url: function () {
      if (this.cardsSideways) {
        return './src/renderer/assets/' + this.cards[this.card].file90
      } else {
        return './src/renderer/assets/' + this.cards[this.card].file
      }
    }
  }
}
</script>

<style>
  .card {
    float: left;
    margin: 1px;
    width: 45px;
  }
  .cardImage {
    width: 45px;
    height: 182px;
  }
  .cardsSideways {
    /* transform: rotate(90deg); */
    height: 45px;
    width: 182px;
  }
  .rotated {
    width: 182px;
  }
  .topLabel {
    background-color: #333;
    color: white;
    font-weight: bold;
    margin: 0px;
    padding: 1px 0px 1px 0px;
  }
  .BLNK {
    background-color: black;
  }
  .IN {
    background-color: #95B0FB;
  }
  .OUT {
    background-color: #1AC161;
  }
  .MVR {
    background-color: #459660;
  }
  .VPU {
    background-color: #101010;
  }
  .EXP {
    background-color: #DBA31F;
  }
</style>
