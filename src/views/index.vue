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
            <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">
              <el-row>
                <div class="bar">
                  <div class="title2"> {{type}}进士的时间分布图</div>
                  <bar v-bind:country="type" v-bind:year="props.year"></bar>
                </div>

              </el-row>
              <el-row>
                <div class="bar">
                  <div class="title2"> {{type}}进士的时间分布图</div>
                  <bar v-bind:country="type" v-bind:year="props.year"></bar>
                </div>

              </el-row>
              <el-row>
                <div class="bar">
                  <div class="title2"> {{type}}进士的时间分布图</div>
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


    export default {
        components:{
            Map,
            bar
        },
        data() {
            return {
                type:"浙江",
                note: {
                    // backgroundImage: "url(" + require("../assets/img/bg.jpg") + ")",
                    backgroundSize: "100% 100%",
                },
                box: {
                    margin:"10px 10px 10px 10px",
                    height: "2rem",
                    border: "0.25rem solid transparent",
                    // borderImage: "url("+require("../assets/img/border.png")+") 51 32 18 66",
                },
                date: new Date(),
                fullscreen: false,
                // year:0
                props:{year: 0}
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
