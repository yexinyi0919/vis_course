<template>
  <div>
    <Echart
        id="bar"
        :options="options"
        height="3rem"
    ></Echart>
  </div>
</template>
<script>
import Echart from '@/common/echart';
import num_data from './num_data.json';
import data1 from './time_be_king.json';
let year_ = 0;

export default {
  name: "index",
  props:{
    country: {   //此处为接收父类传递的参数，进行点击切换数据，默认值为北京
      type: String
    },
    year:{
      type: Number
    },
  },
  watch:{  //监听country值的变化
    country:{
      handler(newValue,oldValue){
        this.country=newValue;
        this.initChart(); }
    },
    year:{
      handler(newValue,oldValue){
      // print(newValue);
        this.year=newValue;
        this.initChart(); }
    }
  },
  data() {
    return {
      options: {},
    };
  },
  components: {
    Echart,
  },
  methods: {
    initChart(){
      let data;
      // if(this.country in num_data) {data = num_data[this.country];}
      // else {data = [0,0,0,0,0,0,0,0,0];}
      data = num_data
      year_ = this.year;

      this.options =  {

        grid: {
          left: '20%',
          bottom:'10%'
        },
        xAxis: [
          {
            type: 'category',
            data: ['清世祖', '清圣祖', '清世宗', '清高宗', '清仁宗', '清宣宗', '清文宗', '清穆宗', '清德宗']
          }
        ],
        yAxis: [
          {
            type: 'value',
            name: '进士人数',
            min: 0,
            max: 6000,
            position: 'left',
            axisLine: {
              show: true
            },
            axisLabel: {
              formatter: '{value}'
            },
            splitLine: {
              show: false
            }
          },
          {
            type: 'value',
            name: '在位时长（年）',
            min: 0,
            max: 70,
            position: 'right',
            axisLine: {
              show: true,
            },
            splitLine: {
              show: false
            },
            axisLabel: {
              formatter: '{value}'
            },
          }
        ],
        series: [
          {
            type: 'bar',
            data: data,
            itemStyle: {
              color: function (params) {
                let colorList = ['#00b090', '#00b090', '#00b090','#00b090', '#00b090', '#00b090','#00b090', '#00b090', '#00b090'];
                colorList[year_] = '#37A2DA';
                // if(year_ !== 0)
                // {
                //   colorList[year_-1] = '#37A2DA';
                // }
                return colorList[params.dataIndex];
              }
            },
          },
          {
            type: 'bar',
            data: data1,
            yAxisIndex: 1,
            itemStyle: {
              color: function (params) {
                let colorList = ['#90f000', '#90f000', '#90f000','#90f000', '#90f000', '#90f000','#90f000', '#90f000', '#90f000'];
                colorList[year_] = '#909000';
                // if(year_ !== 0)
                // {
                //   colorList[year_-1] = '#37A2DA';
                // }
                return colorList[params.dataIndex];
              }
            },
          }
        ]
      }
    }
  },
  mounted() {
    this.initChart();
  },
}
</script>