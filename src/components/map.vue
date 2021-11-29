<template>
<div id="Container">
   <div class="mapContainer" ref="chart">
  </div>
</div>
</template>

<script>
import $ from 'jquery'
import Type from '../../static/学校层次.json'
import china from '../../static/china.json'
import location from '../../static/本科学校经纬度.json'
import yiBenLocation from '../../static/一本科学校经纬度.json'
let echarts = require("echarts");
import PubSub from 'pubsub-js';
export default {
  name: 'mymap',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
    }
  },
  mounted(){
    this.drawLine();
  },
  methods: {
  
   drawLine(){
    // 基于准备好的dom，初始化echarts实例
    var mydata=location
  //  mydata= [{"name":"清华大学","value":[104.087549,30.40824]},{"name":"北京大学","value":[116.316833,39.998877]},{"name":"复旦大学","value":[121.742955,31.066658]}]
    var bar_dv = this.$refs.chart;
        if (bar_dv){
          //console.log('bar_dv不为空');
          let myChart = this.$echarts.init(bar_dv)
          echarts.registerMap("china", china);

      var option = {
        baseOption: {
          backgroundColor: "white",//76becc
          // title: {
          //   text: "全国大学地图",
          //   x: "center",
          //   textStyle: {
          //       fontSize: 20,
          //       color: "#fff"//设置标题字体颜色
          //       },
          // },
          tooltip: {
            show:false
          },
          geo: {
            map: "china",
            center: [108.9199, 33.1904],
            zoom: 1.2,
            itemStyle: {
              shadowBlur: 5,
              normal: {//地图背景色
                        areaColor: '#7fbbe8',//#83a1fd
                        //opacity: 0.5                    
                    },
              emphasis: {//鼠标放上去区域背景色
                        areaColor: "#e9db0b",
                        //opacity: 0.5
                    }
            },
            roam: true,
            scaleLimit: {
              min: 1,
              max: 15
            }
          },
          //触碰文字
          tooltip: {
            trigger: "item",
          },
          backgroundColor: "white",//地图背景色
          series: [
            {
              name:'院校名称',
              type: "scatter",
              coordinateSystem: "geo",
              data: yiBenLocation,
              symbolSize: 15,
              label: {
                normal: {
                  show: false
                },
                emphasis: {
                  show: false
                }
              },
              tooltip:{
                formatter:'{b}'
              },
              itemStyle: {
               normal: {
                            
                            //color: 'red', //散点颜色
                            color:function(pointcolor){
                              for(let i=0;i<Type.length;i++)
                              {
                                if(Type[i]['办学层次']=='专科')
                                {
                                  pointcolor='#eab026'
                                }
                                if(Type[i]['办学层次']=='本科')
                                {
                                  pointcolor='#e3852b'
                                }
                                if(Type[i]['办学层次']=='211')
                                {
                                  pointcolor='#d85d2a'
                                }
                                if(Type[i]['办学层次']=='985')
                                {
                                  pointcolor='#ce2626'
                                }
                              }
                              return pointcolor;
                            },
                            fontSize: '1px'
                }
              }
            }
          ]
        }
      };
      myChart.setOption(option);
      myChart.on("click",function(e){
		  //点击地图板块
        if(e.componentType == "geo") {
            PubSub.publish("pub_mapProvince", e["region"]["name"]);
            console.log(e["region"]["name"])
        }
        //点击地图学校点触发订阅事件
        else{
          PubSub.publish("clickSchool", e["data"]["name"]);
          console.log( e["data"]["name"])
        }
      });

      var myZoom
      var oldZoom=2
      myChart.on('georoam',function(paramas){
        if(paramas.dy||paramas.dx) return
        var _option=myChart.getOption();
        var _zoom=_option.geo[0].zoom
        // if(myZoom==_zoom) return
        myZoom=_zoom
        if(myZoom>2){
          _option.series[0].data=mydata
        }
        else{
          _option.series[0].data=yiBenLocation
        }
        myChart.clear();
        myChart.setOption(_option);
      });
}
}}}
</script>

<style scoped>
.mapContainer{
  width: 100%;
  height: 100%;
}
#Container{
  width: 100%;
  height: 100%;
  //background: url('../assets/地图底图3.jpg') center !important;
}
.border-box{
            position: relative;
            margin:4px auto;
            /* width:400px; */
            height:30px;
            background: rgba(1, 19, 67, 0.8);
            border: 2px solid #00a1ff;
            border-radius: 8px;
            /* z-index: 999; */
        }
        .border-box::before {
            position: absolute;
            top: -2px;
            bottom: -2px;
            left: 30px;
            width: calc(100% - 60px);
            content: "";
            border-top: 2px solid #016886;
            border-bottom: 2px solid #016886;
            z-index: 0;
        }
        .border-box::after {
            position: absolute;
            top: 30px;
            right: -2px;
            left: -2px;
            height: calc(100% - 60px);
            content: "";
            border-right: 10px solid #016886;
            border-left: 10px solid #016886;
            z-index: 0;
        }
        .border-box p{
            /* line-height:100px; */
            font-size: 20px;
            font-weight: bold;
            text-align: center;
            color:#fff;
        }
</style>
