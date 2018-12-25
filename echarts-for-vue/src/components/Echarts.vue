<template>
  <div id="echarts-dom"></div>
</template>

<script>
  import echarts from 'echarts'

  export default {
    name: 'echarts',
    props: {
      chartHeight: Number,
      chartWidth: Number
    },
    data() {
      return {
        myEcharts: {},
        option: {},
        xdata: [],
        ydata: []
      }
    },
    watch: {
      chartHeight(val){
        this.chartHeight = val;
      },
      chartWidth(val){
        let mainDOM = document.getElementById('echarts-dom')
        var echartsDOM = document.createElement("div");    
        echartsDOM.style.height=this.chartHeight+'px'; 
        echartsDOM.style.width=val+'px';
        mainDOM.appendChild(echartsDOM); 
        this.myEcharts = echarts.init(echartsDOM);
        this.option.grid={
          x: val*0.09, 
          y: this.chartHeight*0.12, 
          height: this.chartHeight*0.7,
          width: val*0.9        
        };
        console.log('2',this.chartHeight)
        console.log('2',this.chartWidth)
      }
    },
    mounted() {
      this.initData();
      this.option = {
        title: {
          text: 'ECharts 入门示例',
          textStyle: {
            fontSize: 18,
            fontWeight: 'bolder',
            color: '#333'
          },
          subtext: '纯属虚构',
          subtextStyle: {
            color: '#aaa'
          },
          x: 'center'
        },
        toolbox: { // 工具箱，每个图表最多仅有一个工具箱
          show: true,
          orient: 'vertical', // 布局方式，默认为水平布局，可选为： 'horizontal' ¦ 'vertical'
          x: 'right', // 水平安放位置，默认为全图右对齐，可选为：'center' ¦ 'left' ¦ 'right' ¦ {number}(x坐标，单位px)
          y: 'top'|'-12', // 垂直安放位置，默认为全图顶端，可选为：'top' ¦ 'bottom' ¦ 'center' ¦ {number}(y坐标，单位px)
          borderColor: '#ccc', // 工具箱边框颜色
          borderWidth: 0, // 工具箱边框线宽，单位px，默认为0(无边框)
          padding: 10, // 工具箱内边距，单位px，默认各方向内边距为5，
          itemGap: 10, // 各个item之间的间隔，单位px，默认为10，横向布局时为水平间隔，纵向布局时为纵向间隔
          itemSize: 18, // 工具箱icon大小，单位(px)
          showTitle: true,
          feature: {
            magicType: {
              show: true,
              title: {
                line: '动态类型切换-折线图',
                bar: '动态类型切换-柱形图',
              },
              type: ['line', 'bar']
            },
            restore: {
              show: true,
              title: '还原',
              color: 'black'
            },
            saveAsImage: {
              show: true,
              title: '保存为图片',
              type: 'jpeg',
              lang: ['点击本地保存'] 
            }
          }
        },
        tooltip: {}, // 提示框,鼠标悬浮交互时的信息提示
        legend: { // 图例,每个图表最多仅有一个图例
          orient: 'horizontal',
          x: 'left',
          y: 'top',
          selected: {
            '降水量': false
          },
          data: [ // 使用根据该值索引series中同名系列所用的图表类型和itemStyle，如果索引不到，该item将默认为没启用状态。
            {
              name:'销量',
              textStyle:{fontWeight:'bold', color:'auto'}
            },
            {
              name:'降水量',
              textStyle:{fontWeight:'bold', color:'auto'}
            },
          ]
        },
        // 值域选择，每个图表最多仅有一个值域控件
        dataRange: {
          show: true,
          color: ['red', 'orange', 'yellow'],
          splitList: [
            {start: 40},
            {start: 30, end: 40},
            {start: 20, end: 30},
            {start: 10, end: 20},
            {end: 10, /*label: '10(自定义特殊颜色)', color: 'black'*/}
          ]
          /*min: 0,
          max: 40,
          calculable: true,
          formatter: function(v) {
            if (v > 35) {return v + ' (高)'}
            else if (v < 10) {return v + ' (低)'}
            else {return v + ' (中)'};
          }*/
        },
        // 数据区域缩放,仅对直角坐标系图表有效
        dataZoom: {
          show: true,
          realtime: true,
          orient: 'horizontal',
          x: this.charWidth*0.24,
          y: 'bottom'|this.charHeight*0.94,
          height: 20,
          start: 10, // 数据缩放，选择起始比例，默认为0(%)，从首个数据起选择
          end: 90 // 数据缩放，选择结束比例，默认为100(%)，到最后一个数据选择结束
        },
        grid:{
          x: this.charWidth*0.25, // 直角坐标系内绘图网格左上角横坐标，数值单位px，支持百分比(字符串)，如'50%'(显示区域横向中心)
          y: this.charHeight*0.16, // 直角坐标系内绘图网格左上角纵坐标，数值单位px，支持百分比(字符串)，如'50%'(显示区域纵向中心)
          x2: 10, // 直角坐标系内绘图网格右下角横坐标，数值单位px，支持百分比(字符串)，如'50%'(显示区域横向中心)
          y2: 20, // 直角坐标系内绘图网格右下角纵坐标，数值单位px，支持百分比(字符串)，如'50%'(显示区域纵向中心)
          height: this.charHeight*0.7,
          width: this.charWidth*0.6
        },
        xAxis: [
          {
            type: 'category', // 坐标轴类型，横轴默认为类目型'category'，纵轴默认为数值型'value'
            show: true,
            z:1,
            position: 'bottom', // 横轴默认为类目型'bottom'，纵轴默认为数值型'left'，可选为：'bottom' | 'top' | 'left' | 'right'
            name: 'X轴', // 坐标轴名称，默认为空
            nameLocation: 'end', // 坐标轴名称位置，默认为'end'，可选为：'start' | 'end'
            nameTextStyle: { // 坐标轴名称文字样式
              fontSize: 15,
              fontWeight: 'bolder',
              color: 'red'
            },
            boundaryGap: true, // 类目起始和结束两端空白策略，默认为true留空，false则顶头
            axisLine: {    // 坐标轴线，默认显示
              show: true,
              lineStyle: {
                color: 'green', 
                type: 'dotted', // 线条样式，可选为：'solid' | 'dotted' | 'dashed'， 树图还可以选：'curve' | 'broken'
                width: 2
              }
            },
            axisTick: {    // 坐标轴小标记，默认不显示
              show:true,
              length: 10,
              lineStyle: {
                color: 'red',
                type: 'curve',
                width: 2
              }
            },
            axisLabel: { // 坐标轴文本标签选项
              show:true,
              interval: 'auto',
              rotate: 45,
              margin: 8,
              formatter: function(v) {
                return v;
              },
              textStyle: {
                color: 'blue',
                fontFamily: 'sans-serif',
                fontSize: 5,
                fontStyle: 'italic', // 样式，可选为：'normal' | 'italic' | 'oblique'
                fontWeight: 'bold' // 粗细，可选为：'normal' | 'bold' | 'bolder' | 'lighter' | 100 | 200 |... | 900
              }
            },
            splitLine: { // 分隔线，默认显示
              show:true,
              lineStyle: {
                color: '#71C671',
                type: 'dotted',
                width: 3
              }
            },
            splitArea: { // 分隔区域，默认不显示
              show: true,
              areaStyle:{
                color:['rgba(144,238,144,0.3)','rgba(135,200,250,0.3)','rgba(224,255,255,0.3)']
              }
            },
            data: [
              '衬衫', '羊毛衫', '雪纺衫', 
              {
                value:'裤子',
                textStyle: {
                  color: 'red',
                  fontSize: 20,
                  fontStyle: 'normal',
                  fontWeight: 'bold'
                }
              },
              '高跟鞋', '袜子'
            ] // 类目列表，同时也是label内容，若为空可以通过axisLabel的formatter进行设置 => '{value}月'
          },
          {
            type: 'category',
            z:1,
            position: 'top',
            data: ['One','Tow','Three','Four','Five']
          }
        ],
        yAxis: [
          {
            position: 'left', // 横轴默认为类目型'bottom'，纵轴默认为数值型'left'，可选为：'bottom' | 'top' | 'left' | 'right'
            name: 'Y轴(销售量)', // 坐标轴名称，默认为空
            nameLocation: 'end', // 坐标轴名称位置，默认为'end'，可选为：'start' | 'end'
            nameTextStyle: { // 坐标轴名称文字样式
              fontSize: 15,
              fontWeight: 'bolder',
              color: 'red'
            },
            axisLabel: {
              show:true,
              interval: 'auto',
              rotate: -20, // 旋转角度，默认为0，不旋转，正值为逆时针，负值为顺时针，可选为：-90 ~ 90 
              margin: 18,
              formatter: '{value} 个',
              textStyle: {
                color: '#1e90ff',
                fontFamily: 'verdana',
                fontSize: 10,
                fontStyle: 'normal',
                fontWeight: 'bold'
              }
            }
          },
          {
            type: 'value',
            splitNumber: 10, // 分割段数，默认为5，为0时为线性渐变，calculable为true是默认均分100份
            axisLabel: {
              formatter: function (value) {
                return value + ' °C'
              }
            },
            splitLine: {
              show: false
            }
          }
        ],
        series: [ // 驱动图表生成的数据内容数组，数组中每一项为一个系列的选项及数据
          {
            name: '销量',
            type: 'bar', // 图表类型，必要参数！如为空或不支持类型，则该系列数据不被显示。可选为：'line'(折线图)|'bar'(柱状图)|'scatter'(散点图)|'k'(K线图)'pie'(饼图)|'radar'雷达图)|'chord'(和弦图)|'force'(力导向布局图)|'map'(地图)
            tooltip: { // 提示框，鼠标悬浮交互时的信息提示
              show: true,
              showContent: true, // tooltip主体内容显示策略，只需tooltip触发事件或显示axisPointer而不需要显示内容时可配置该项为false，可选为：true(显示) | false(隐藏)
              trigger: 'item', // 触发类型，默认数据触发，可选为：'item' | 'axis'
              position: function(p) { // 返回相当鼠标的位置，可进行重新定位
                return [p[0] + 20, p[1] - 20];
              },
              formatter: function (params,ticket,callback) { // 显示交互信息的格式化
                return 'loading';
              },
              textStyle: {
                color: 'yellow',
                decoration: 'none',
                fontFamily: 'Verdana, sans-serif',
                fontSize: 15,
                fontStyle: 'italic',
                fontWeight: 'bold'
              },
              backgroundColor: 'rgba(100,50,0,0.7)', // 提示背景颜色
              borderWidth: 2, // 提示边框线宽，单位px，默认为0
              borderColor: 'black', // 提示边框颜色
              padding: 10, // 提示内边距，单位px，默认各方向内边距为5，接受数组分别设定上右下左边距，同css
              borderRadius: 15, // 提示边框圆角，单位px，默认为4
            },
            data: [5, 20, 35, 10, 45, 20], // 当某类目对应数据不存在时(ps：'不存在' 不代表值为 0)，可用'-'表示，无数据在折线图中表现为折线在该点断开，在柱状图中表现为该点无柱形
            markPoint: { // 系列中的数据标注内容
              symbol: 'pin', // circle'|'rectangle'|'triangle'|'diamond'|'emptyCircle'|'emptyRectangle'|'emptyTriangle'|'emptyDiamond'另外，还支持五种更特别的标志图形'heart'(心形)、'droplet'(水滴)、'pin'(标注)、'arrow'(箭头)和'star'(五角星)
              symbolRotate: null, // 标注图形旋转角度
              effect: {}, // 标注图形炫光特效
              data: [
                {type: 'max', name: '最大值'},
                {type: 'min', name: '最小值'}
              ]
            },
            markLine: { // 系列中的数据标线内容
              symbol: ['circle', 'arrow'], // 标线起始和结束的symbol介绍类型，如果都一样，可以直接传string
              symbolSize: [10, 15], // 标线起始和结束的symbol大小，半宽(半径)参数，如果都一样，可以直接传number或function
              itemStyle: {
                normal: {                   // 系列级个性化，横向渐变填充
                  borderRadius: 5,
                  color: 'red',
                  label: {
                      show: true,
                      textStyle: {
                          fontSize: '15',
                          fontFamily: '微软雅黑',
                          fontWeight: 'bold'
                      }
                  }
                }
              },
              data: [
                {type: 'average', name: '平均值'}
              ]
            },
            itemStyle: {
              normal: { // 默认样式
                color: '#FF1493',
                barBorderColor: 'blue', // 柱形图 边框颜色
                barBorderRadius: 20, // 柱形图 柱形边框圆角，单位px，默认为0，支持传入数组分别指定柱形4个圆角半径，如:[5, 5, 0, 0](顺时针左上，右上，右下，左下)
                barBorderWidth: 2, // 柱形图 柱形边框线宽，单位px，默认为0
                label: { // 标签
                  show: true,
                  //position:'insideBottom' // 柱形图可选的还有，'insideLeft' | 'insideRight' | 'insideTop' | 'insideBottom'
                },
              },
              emphasis: { // 强调样式
                color: '#FFBBFF'
              }
            }
          },
          {
            name: '降水量',
            type: 'bar',
            data: [10, 25, 40, 15, 50, 25]
          },
          {
            name:'最高气温',
            type: 'line',
            z: 3,
            xAxisIndex: 1, // 默认：0 ,xAxis坐标轴数组的索引，指定该系列数据所用的横坐标轴
            yAxisIndex: 1, // 默认：0 ,yAxis坐标轴数组的索引，指定该系列数据所用的纵坐标轴
            data: [12.0, 12.2, 13.3, 14.5, 16.3]
          },
          {
            name:'最低气温',
            type: 'line',
            z: 3,
            yAxisIndex: 1,
            xAxisIndex: 1,
            data: [2.0, 2.2, 3.3, 4.5, 6.3]
          }
        ]
      }
    },
    methods:{
      initData(){
        setTimeout(() => this.getData(), 1000);
      },
      getData(){
        this.option.xAxis[0].data = ['666','666','666','666','666','666'];
        this.option.series[0].data = [666,666,666,666,666,666,];
        this.myEcharts.setOption(this.option);
      }
    }
  }
</script>