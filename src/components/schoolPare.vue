<template>
    <div class="school" ref="school" >
    </div>
</template>

<script>
import $ from 'jquery'
import schoolData from '../../static/schoolData.json'
import mydata from '../../static/平行坐标轴区域划分.json'
let echarts = require("echarts");

export default {
  name: 'school',
  data () {
    return {
        location:'四川'
    }
  },
mounted(){
    this.drawLine();

  },
  methods: {
   drawLine(){
       

var schema = [
    {name: '学校层次', index: 1, text: '学校层次'},
    {name: '学校类型', index: 2, text: '学校类型'},
    {name: '学校所在省份', index: 3, text: '学校所在区域'},
    {name: '最低录取分数', index: 4, text: '最低录取分数'},
    {name: '最低录取排名', index: 5, text: ' 最低录取排名'},
    {name: '2019录取人数', index: 6, text: '2019录取人数'},
    {name: '地区GDP', index: 7, text: '地区GDP'}

];

var lineStyle = {
    normal: {
        width: 1,
        opacity: 0.5
    }
};
    

var option = {
    title: {
            text: '院校综合信息平行坐标轴',
            left: "center",
            textStyle: {
                fontSize: 20,
                color: "black"//设置标题字体颜色#fff
                },
        },
    backgroundColor: 'white',/*#081130*/
    legend: {
        left: 'right',
        data: ['文科', '理科'],
        itemGap: 20,
        textStyle: {
            color: 'black',
            fontSize: 14
        }
    },
    tooltip: {
        padding: 10,
        backgroundColor: '#222',
        borderColor: '#777',
        borderWidth: 1
    },
    // dataZoom: {
    //     show: true,
    //     orient: 'vertical',
    //     parallelAxisIndex: [0]
    // },
    parallelAxis: [
        {dim: 0, name: schema[0].text, type: 'category', data: ['985', '211', '普通本科', '专科'], nameLocation: 'start'},
        {dim: 1, name: schema[1].text,
         type: 'category', data:['综合','财经','工科','艺术','师范','体育','政法','农业','医药','民族','语言','林业']},
        {dim: 2, name: schema[2].text,
         type: 'category', data: ['东北','华北','华东','华南','华中','西北','西南']},
         //[17041.60,23739.61,387437.67,47819.33,46740.33,11539.70,30315.75],按照上面的顺序来的
        {dim: 3, name: schema[3].text},
        {dim: 4, name: schema[4].text},
        {dim: 5, name: schema[5].text},
        {dim: 6, name: schema[6].text,type: 'category', data:[17041.60,23739.61,387437.67,47819.33,46740.33,11539.70,30315.75]
        },
    ],
    
    parallel: {
        left: '5%',
        right: '18%',
        bottom: 20,
        parallelAxisDefault: {
            type: 'value',
            // name: '学校层次',
            nameLocation: 'end',
            nameGap: 20,
            nameTextStyle: {
                color: 'black',
                fontSize: 12,
            },
            axisLine: {
                lineStyle: {
                    color: 'black'/*#FFCCCC*/
                }
            },
            axisTick: {
                lineStyle: {
                    color: 'black'/*#777*/
                }
            },
            splitLine: {
                show: false
            },
            axisLabel: {
                color: 'black'/*#fff*/
            }

        }
    },
    series: [
        {
            name: '理科',
            type: 'parallel',
            lineStyle: lineStyle,
            data:  mydata['理科'],
            smooth: true,
            lineStyle: {
            width: 0.2,
            color: "rgba(255, 52, 110, 0.5)"
        }
        },
        {
            name: '文科',
            type: 'parallel',
            lineStyle: lineStyle,
            data:// mydata['文科'],
        mydata[["985","综合","华北",658,38,23,"北京大学",23739.61],
    ["985","综合","华北",646,106,41,"中国人民大学",23739.61],
    ["985","综合","华北",666,35,2,"清华大学",23739.61],
    ["211","工科","华北",610,1387,7,"北京交通大学",23739.61],
    ["985","工科","华北",626,534,2,"北京航空航天大学",23739.61],
        ["985","工科","华北",621,729,2,"北京理工大学",23739.61],
        ["211","工科","华北",600,2228,22,"北京科技大学",23739.61],
        ["211","工科","华北",588,3795,11,"北京化工大学",23739.61],
        ["普通本科","财经","华北",588,3795,7,"北京工商大学",23739.61],
        ["211","工科","华北",603,1942,10,"北京邮电大学",23739.61],
        ["985","农业","华北",610,1387,14,"中国农业大学",23739.61], 
            ["211","综合","东北",587,3952,30,"辽宁大学",17041.60],
        ["普通本科","工科","东北",549,14013,8,"沈阳工业大学",17041.60],
        ["普通本科","工科","东北",533,21480,4,"沈阳航空航天大学",17041.60],
        ["普通本科","工科","东北",529,23701,5,"沈阳理工大学",17041.60],
        ["985","工科","东北",594,2958,15,"东北大学",17041.60],
        ["普通本科","工科","东北",545,15679,10,"辽宁工程技术大学",17041.60],
        ["普通本科","工科","东北",540,17984,2,"大连交通大学",17041.60],
        ["211","工科","东北",585,4253,14,"大连海事大学",17041.60],
        ["普通本科","师范","东北",540,17984,45,"辽宁师范大学",17041.60]
         ],
          smooth: true,
            lineStyle: {
            width: 0.2,
            color: "rgba(103, 85, 213, 0.5)"
        }
        },
        
    ]
};







    // 基于准备好的dom，初始化echarts实例
    var bar_dv = this.$refs.school;
        if (bar_dv){
          let myChart = this.$echarts.init(bar_dv)
      myChart.setOption(option);
}
}}}
</script>

<style scoped>

.school{
  width: 100%;
  height: 270px;
}


</style>
