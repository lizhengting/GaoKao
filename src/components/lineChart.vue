<template>
    <div v-show="(school!='')&&(location=='')" >
        <div class="lineChart" ref="lineChart"></div>
        <div class="barChart" ref="barChart"></div>
    </div>
</template>

<script>
import $ from 'jquery'
import mydata from '../../static/理科历年批次分数2.json'
import wenke from '../../static/文科历年批次分数2.json'
import dataAll from '../../static/文理科批次分数线.json'
import PubSub from 'pubsub-js'
let echarts = require("echarts");

export default {
  name: 'enter',
  data () {
    return {
        location:'四川',
        school:''
    }
  },
mounted(){
    PubSub.subscribe("clickSchool", (msg, data) => {
      this.school = data;
      this.location=''
      this.drawLineChart(data)
      this.drawBarChart(data)
    });
    PubSub.subscribe("pub_mapProvince", (msg, data) => {
       this.school=''
       this.location=data
       
    });
  },
  methods: {
   drawLineChart(schoolName){
    // 基于准备好的dom，初始化echarts实例
    var year 
    var index
    var lineData=[]
    var myLegend=[]
    // console.log(mydata[0][nam])
    for(let j=0;j<dataAll.length;j++)
    {
        if(dataAll[j]['学校']==schoolName)
        {
            for(let i=0;i<dataAll[j]['文科']['lable'].length;i++)
            {
                let x=dataAll[j]['文科']['lable'][i]
                 var obj={
                    name: x,
                    type: 'line',
                   // stack: '总量',
                    data:dataAll[j]['文科'][x]['最低分数线'].reverse()
                }
                myLegend.push(x)
                lineData.push(obj)
            }
            for(let i=0;i<dataAll[j]['理科']['lable'].length;i++)
            {
                let x=dataAll[j]['理科']['lable'][i]
                 var obj={
                    name: x,
                    type: 'line',
                   // stack: '总量',
                    data:dataAll[j]['理科'][x]['最低分数线'].reverse()
                }
                myLegend.push(x)
                lineData.push(obj)
            }
            break;

        }

    }
    let siChuanLikeYiPi=[532,511,546,547]
    let siChuanWenkeYiPi=[540,537,553,540]
    let wenKeobj={
        name: '文科本科一批次收分线',
        type: 'line',
        smooth:false,   //关键点，为true是不支持虚线的，实线就用true
            itemStyle:{
                normal:{
                    lineStyle:{
                        width:2,
                        type:'dotted'  //'dotted'虚线 'solid'实线
                    }
                }
            },            
            data:siChuanWenkeYiPi
    }
    let liKeObj={
        name: '理科本科一批次收分线',
        type: 'line',
        smooth:false,   //关键点，为true是不支持虚线的，实线就用true
            itemStyle:{
                normal:{
                    lineStyle:{
                        width:2,
                        type:'dotted'  //'dotted'虚线 'solid'实线
                    }
                }
            },            
            data:siChuanLikeYiPi
    }

    lineData.push(wenKeobj,liKeObj)
    // console.log(lineData)

    var bar_dv = this.$refs.lineChart;
        if (bar_dv){
          let myChart = this.$echarts.init(bar_dv)
   var option = {
    title: {
        text: '历年最低录取分数线',
        left: "center",
                textStyle: {
                fontSize: 20,
                color: "black"//设置标题字体颜色
                 },
    },
    tooltip: {
        trigger: 'axis',
        show:'ture'
    },
    legend: {
        data:myLegend.reverse(),
        bottom:"2%",
        textStyle: { //图例文字的样式
                            color: 'black',
                            fontSize: 16
                        }
    },
    grid: {
        left: '3%',
        right: '4%',
        bottom: '15%',
        width:'85%',
        containLabel: true
    },
    toolbox: {
        feature: {
            saveAsImage: {}
        }
    },
    xAxis: {
        type: 'category',
         nameTextStyle:{
            fontWeight: "bolder",
            color: "black",//rgba(0, 0, 0, 1)
            fontSize :12
        },
         axisLine: {
                lineStyle: {
                    color: "black",
                }
            },
        boundaryGap: true,
        data: ["2019","2018","2017","2016"].reverse()
    },
    yAxis: {
        type: 'value',
        scale:true,
         axisLine: {
                lineStyle: {
                    color: "black",
                }
            },
    },
    series: lineData
};
      myChart.setOption(option);
    }
    },
    
//柱状图
    drawBarChart(schoolName){
        var barData=[]
    // console.log(mydata[0][nam])
//     for(let i=0;i<mydata[item]['批次'].length;i++)
//     {
//         var nam=mydata[item]['批次'][i]
//         var obj={
//             data:mydata[item][nam][2],
//             type: 'bar',
//             showBackground: true,
//             backgroundStyle: {
//                 color: 'rgba(180, 180, 180, 0.2)'
//             },
//             itemStyle: {
//             normal: {
// 　　　　　　　　//这里是重点
//                 color: function(params) {
//                 	//注意，如果颜色太少的话，后面颜色不会自动循环，最好多定义几个颜色
//                     var colorList = [ '#61a0a8', '#d48265', '#91c7ae','#749f83', '#ca8622'];
//                     return colorList[params.dataIndex]
//                     }
//                 }
//             }
//         }
//         barData.push(obj)
//     }

var myLegend=[]
for(let j=0;j<dataAll.length;j++)
    {
        if(dataAll[j]['学校']==schoolName)
        {
            for(let i=0;i<dataAll[j]['文科']['lable'].length;i++)
            {
                let x=dataAll[j]['文科']['lable'][i]
                barData.push(dataAll[j]['文科'][x]['录取人数'])
                myLegend.push(x)
            }
            for(let i=0;i<dataAll[j]['理科']['lable'].length;i++)
            {
                let x=dataAll[j]['理科']['lable'][i]
                barData.push(dataAll[j]['理科'][x]['录取人数'])
                myLegend.push(x)
            }
            break;
        }

    }
    var data = [];
    for (var i = 0; i < barData.length; i++) {
        for (var j = 0; j < barData[i].length; j++) {
            var label = myLegend[i];
            data.push([
                j, barData[i][j], label
            ]);
        }
    }

    var bar_dv = this.$refs.barChart;
        if (bar_dv){
          let myChart = this.$echarts.init(bar_dv)
   var option = {
            title: {
            text: '近年录取人数',
            left: "center",
            textStyle: {
                fontSize: 20,
                color: "black",//设置标题字体颜色
                fontWeight:"bold"
                },
        },
        singleAxis: {
                max: 'dataMax'
            },
        grid: {
        top:'5%',
        left: '3%',
        right: '4%',
        bottom: '15%',
        width:'85%',
        containLabel: true
    },
            series: [{
                type: 'themeRiver',
                data: data,
                color:['#0e8dcc','#29a8ee','#20c6dc','#02337f','#d59d4f'],
                label: {
                    show: true,
                    textStyle: { //图例文字的样式
                            color: 'black',
                            fontSize: 16
                        }
                },
                itemStyle: {
                color: ['#8CC7B5','#A0EEE1', '#BEEDC7']
    }
            }],

};
      myChart.setOption(option);
    }
    }

}}
</script>

<style scoped>
.lineChart{
  width: 350px;
  height: 250px;
  margin-top: 10%
}
.barChart{
  width: 350px;
  height: 270px;
  margin-top: 3%
}
</style>
