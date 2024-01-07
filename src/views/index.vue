<template>
    <div class="index" :style ="note">
        <el-row>
            <el-col :xs="{span:12,offset:6}" :sm="{span:12,offset:6}" :md="{span:12,offset:6}" :lg="{span:16,offset:4}" :xl="{span:16,offset:4}">
                <div class="visualization">清代进士时空分布可视化</div>
            </el-col>
        </el-row>



        <el-row>
            <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">
                <Map @updateCity="updateCity" @updateTime="updateTime" style="margin: 0.125rem;"></Map>
            </el-col>


          

            <el-col :xs="24" :sm="24" :md="6" :lg="6" :xl="6">
              <el-row>
                <div class="pie1">
                  <div class="title2"> {{years[props.year]}}的进士地域分布图</div>
                  <pie1 v-bind:country="type" v-bind:year="props.year"></pie1>
                </div>
              </el-row>
            </el-col>

            <el-col :xs="24" :sm="24" :md="6" :lg="6" :xl="6">

              <el-row>
                <div class="pie2">
                  <div class="title2"> {{years[props.year]}}的文武状元地域分布图</div>
                  <pie2 v-bind:country="type" v-bind:year="props.year"></pie2>
                </div>
              </el-row>
            </el-col>


            <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">

            <el-row>
              <div class="lastname">
                <div class="title2"> {{years[props.year]}}时期{{type}}进士姓氏分布图</div>
                <lastname v-bind:country="type" v-bind:year="props.year"></lastname>
              </div>
            </el-row>

          </el-col>


            <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">

              <el-row>
                <div class="bar">
                  <div class="title2"> 全国进士的时间分布图</div>
                  <bar v-bind:country="type" v-bind:year="props.year"></bar>
                </div>
              </el-row>

            </el-col>
        </el-row>
    </div>
</template>

<script>
    import Map from '@/components/map/index'
    import bar from '@/components/bar/index'
    import pie1 from '@/components/pie1/index'
    import pie2 from '@/components/pie2/index'
    import lastname from "@/components/lastname/index.vue";


    export default {
        components:{
            Map,
            bar,
            pie1,
            pie2,
            lastname
        },
        data() {
            return {
                type:"全国",
                note: {
                    // backgroundImage: "url(" + require("../assets/img/bg.jpg") + ")",
                    backgroundSize: "100% 100%",
                },
                box: {
                    margin:"0px 0px 0px 0px",
                    height: "2rem",
                    border: "0.25rem solid transparent",
                    // borderImage: "url("+require("../assets/img/border.png")+") 51 32 18 66",
                },
                date: new Date(),
                fullscreen: false,
                // year:0
                props:{year: 0},
                years: ['清世祖','清圣祖','清世宗','清高宗','清仁宗','清宣宗','清文宗','清穆宗','清德宗'],
            };
        },
        mounted(){
            let that= this;
            this.timer = setInterval(function() {
                that.date = new Date().toLocaleString();
            });
        },
        methods:{
            updateCity(data){         //触发子组件城市选择-选择城市的事件
                this.type = data
            },
          updateTime(data)
          {
            this.props.year = data
          }
        },
        beforeDestroy: function() {
            if (this.timer) {clearInterval(this.timer);}
        }
    }
</script>

<style scoped>
    .index {

    }
    .visualization{
        color: #000000;
        font-size: 20px;
        font-weight: bold;
        margin-bottom: 15px;
        text-align: center;
        margin-top: 0.125rem;
    }
    .title2{
        color: #000000;
        font-size: 17px;
        font-weight: bold;
        text-align: center;
    }
</style>
