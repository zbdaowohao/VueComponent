<template>
  <div id="app" ref="chartDom">
    <Row style="height:100%">
      <Col span="12" style="height:100%">
        <Echarts :chartHeight="chartHeight" :chartWidth="chartWidth"/>
      </Col>
      <Col span="12" style="height:100%;border:red 1px solid;">
        <!-- <echarts-map :chartHeight="chartHeight" :chartWidth="chartWidth"/> -->
      </Col>
    </Row>
  </div>
</template>

<script>
import Echarts from './components/Echarts'
import EchartsMap from './components/EchartsMap'

export default {
  name: 'App',
  components: {
    Echarts,
    EchartsMap
  },
  data() {
    return {
      chartHeight: 0,
      chartWidth: 0
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.chartHeight = this.$refs.chartDom.clientHeight;
      this.chartWidth = this.$refs.chartDom.clientWidth;
      console.log('1',this.chartHeight)
      console.log('1',this.chartWidth)
    });
  }
}
</script>

<style>
#app {
  width: 100%;
  height: 400px;
  padding: 10px;
}
</style>
