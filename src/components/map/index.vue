<template>
  <div class="echart" ref="mapRef" id="map"></div>
</template>

<script>
import chinaJson from './china.json'
import 'echarts/lib/chart/scatter'
import province_to_num from './province_to_num.json'


var cityName = '北京市'
var cur_year = '清世祖'
const years = ['清世祖','清圣祖','清世宗','清高宗','清仁宗','清宣宗','清文宗','清穆宗','清德宗']


export default {
  mounted() {
    this.initChart();
  },
  methods: {
    initChart() {
      this.chart = this.$echarts.init(document.getElementById('map')/*, 'shine'*/);
      this.$echarts.registerMap("china", chinaJson);

      // 指定图表的配置项和数据
      let option = {
        baseOption:{
          timeline: {
            axisType: 'category',
            orient: 'vertical',
            autoPlay: true,
            inverse: true,
            playInterval: 1000,
            left: 30,
            top: 40,
            bottom: "26%",
            width: 55,
            height: null,
            symbol: 'none',
            checkpointStyle: {
              borderWidth: 2
            },
            controlStyle: {
              showNextBtn: false,
              showPrevBtn: false
            },
            data: years
          },
        },
        options:[],

      };

      for (var n = 0; n < years.length; n++) {
        // option.baseOption.timeline.data.push(years[n]);
        option.options.push({
          title: {
            text: years[n]+'进士空间分布图'
          },
          layoutCenter: ['60%', '50%'],//位置
          layoutSize:'80%',//大小
          visualMap: [
            {
              orient: 'vertical',
              min: 0,
              max: 600,
              text: ['进士人数'],
              realtime: false,
              calculable: true,
              bottom:"0%",                              //组件离容器下侧的距离,'20%'
              inRange: {
                color: ['#ffffff',
                  '#00aa88']
              }
            }],
          series: [
            {
              type: "map",
              map: "china",
              data:province_to_num[n]
            },

          ]
        },
        );
      }




      this.chart.setOption(option, true);
      this.chart.on('click', (param)=>{
        cityName = param.name;
        this.$emit('updateCity',cityName);
      });
      this.chart.on('timelinechanged', (param)=>{
        cur_year = param.currentIndex;
        this.$emit('updateTime',cur_year);
      });
    },
  }
}
</script>

<style scoped>
.echart {
  height: 12rem;
}
</style>
