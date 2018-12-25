<template>
  <div id="echarts-map" :style="{height:charHeight+'px',width:charWidth+'px'}"></div>
</template>

<script>
  import echarts from 'echarts'
  import { option } from '../js/map'

  export default {
    name: 'echarts',
    data() {
      return {
        charHeight:500,
        charWidth:800
      }
    },
    mounted() {
      let echartsDOM = document.getElementById('echarts-map')
      let myEcharts = echarts.init(echartsDOM)
      myEcharts.setOption(option);
    }
  }
</script>