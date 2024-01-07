<template>
  <div>
    <Echart
        id="pie2"
        :options="options"
        height="3rem"
    ></Echart>
  </div>
</template>

<script>
import pie2data from './pie2.json'
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
      data = pie2data[this.year];
      //data = [1,2,3,4,5,6];


      let filteredData = data.filter(item => item.value !== 0);

      let colors = new Array(filteredData.length).fill('#00b090');

      // Find the index of the specified country in the data
      let countryIndex = filteredData.findIndex(item => item.name.toLowerCase() === this.country.toLowerCase());


      if (countryIndex !== -1) {
        colors[countryIndex] = '#37A2DA';  // Set the desired highlight color
      }

      let radiusData = filteredData.map((item, index) => {
        const isCountry = index === countryIndex;
        const baseRadius = isCountry ? '60%' : '50%';

        return {
          name: item.name,
          value: item.value,
          label: {
            formatter: '{b}: {c}人\n{d}%',
            show: true,
          },
          radius: baseRadius,
          itemStyle: {
            borderColor: '#fff', // Set the border color to white
            borderWidth: 0.1,      // Set the border width
          },
        };
      });


      this.options = {
        series: [
          {
            type: "pie",
            radius: "50%",
            center: ["50%", "50%"],
            data: radiusData,
            label: {
              formatter: "{b}: {c} 人{d}%",
            },
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
