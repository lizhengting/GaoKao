<template>
    <div class="enterScoreContainer" v-show="(school=='')&&(location!='')">
        <!-- <div class="buttonShow">
            <button class="mybutton">上一年</button> 
         <p>{{location}}历年录取线</p>
        <button class="mybutton">下一年</button>
        </div> -->
        <div class="enterScore" ref="enterScore"></div>
    </div>
</template>

<script>
import $ from 'jquery'
import Score from '../../static/enterScore.json'
let echarts = require("echarts");
import PubSub from 'pubsub-js'
export default {
  name: 'enter',
  data () {
    return {
        location:'四川',
        school:''
    }
  },
mounted(){
    this.drawLine(0);
    PubSub.subscribe("pub_mapProvince", (msg, data) => {
        this.location=data
        this.school=''
        for(let i=0;i<Score.length;i++)
        {
            if(Score[i]['province']==data)
            {
                //console.log('xx',Score[i])
                this.drawLine(i)
                break;
            }
        }
    });
     PubSub.subscribe("clickSchool", (msg, data) => {
      this.school = data;
      this.location=''
    });

  },
  methods: {
   drawLine(index){
    var years=Score[index]['years']
    var province=Score[index]['province']
    var lable=Score[index]['lable']
    // console.log('years',years,'province',province,'lable',lable)
    var data=[]
    var colorList = ['#0e8dcc','#20c6dc', '#2a5caa'];
    for(let i in years)
    {
        let barColor=colorList[i]
        let obj= {
            name: years[i],
            type: 'bar',
            //stack: '总量',
            label: {
                show: true,
                position: 'insideRight'
            },
            data:Score[index][ years[i]],
            itemStyle: {
                                normal: {
                                    color: barColor,  //柱状颜色
                                }
                            }
        }
        data.push(obj)
    }
    // 基于准备好的dom，初始化echarts实例
    var bar_dv = this.$refs.enterScore;
        if (bar_dv){
          let myChart = this.$echarts.init(bar_dv)
    var option = {
    title: {
            text:this.location+ '批次线',
            left: "center",
            textStyle: {
                fontSize: 20,
                color: "black"//设置标题字体颜色
                },
        },
    tooltip: {
        trigger: 'axis',
        axisPointer: {            // 坐标轴指示器，坐标轴触发有效
            type: 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
        },
        
    },
    legend: {
        data:years,
        top:'5%',
         textStyle: {
                fontSize: 15,
                color: "black"//设置标题字体颜色rgba(245, 237, 237, 1)
                 },
    },
    grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        width:'85%',
        containLabel: true
    },
    xAxis: {
        type: 'value',
        nameTextStyle:{
            fontWeight: "bolder",
            color: "rgba(0, 0, 0, 1)",
            fontSize :12
        },
       axisLine: {
                lineStyle: {
                    color: "black",
                }
            }

    },
    yAxis: {
        type: 'category',
        data:lable,
         axisLine: {
                lineStyle: {
                    color: "black",
                }
            }
    },
    series:data
};
      myChart.setOption(option);
}
}}}
</script>

<style scoped>
.enterScore{
  width: 100%;
  height: 540px;
  margin-top: 10%
}
.enterScoreContainer{
    width: 100%;
}
</style>
