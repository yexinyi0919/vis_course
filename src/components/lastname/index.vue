<template>
  <div>
    <Echart
        id="pie1"
        :options="options"
        height="3rem"
    ></Echart>
  </div>
</template>

<script>
import lastname from './lastname.json'
import Echart from "@/common/echart/index.vue";
//import 'echarts/lib/chart/scatter'
//import province_to_num from './province_to_num.json'




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
  components: {Echart},
  methods: {
    initChart() {
      let data;
      data = lastname[this.year];
      //data = [1,2,3,4,5,6];

      let countryIndex = data.findIndex(item => item.name === this.country);

      let selectedCountryData = countryIndex !== -1 ? data[countryIndex].value : { lastname: [], count: [] };


      let colors = new Array(data.length).fill('#4682B4');

      // Find the index of the specified country in the data

      //const COLOR = [
      //  '#b5bd48',
      //  '#8378EA',
      //  '#96BFFF',
      //  '#8696a7',
      //  '#656565',
      //]



      this.options = {
        grid: {
          left: '20%',
          bottom:'10%'
        },
        yAxis: [
          {
            type: 'category',
            data: selectedCountryData.lastname
          }
        ],
        xAxis: [
          {
            type: 'value',
            name: '进士人数',
            min: 0,
            max: 80,
            position: 'left',
            axisLine: {
              show: true
            }
          }
        ],
        series: [
          {
            type: "bar",
            data: selectedCountryData.count.map((count, index) => ({
              name: selectedCountryData.lastname[index],
              value: selectedCountryData.count[index],
            })),
            label: {
              show: true,  // Labels are shown on emphasis for better visibility
              position: 'insideRight',
              formatter: '{c}人',
            },
            //emphasis: emphasisStyle
          }
        ],
        color: colors,
      }
    }
  },
  mounted() {
    this.initChart();
  },
}
</script>
