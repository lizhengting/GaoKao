<template>
    <div>
        <div class="circle" ref="circle"></div>
    </div>
</template>

<script>
import $ from 'jquery'
import Type from '../../static/schoolType.json'
let echarts = require("echarts");
import PubSub from 'pubsub-js';

export default {
  name: 'enter',
  data () {
    return {
        location:'全国',
    }
  },
mounted(){
    this.drawCircle(Type[0]['data']);
    PubSub.subscribe("pub_mapProvince", (msg, data) => {
      this.location = data;
      var temp
      for(let k=0;k<Type.length;k++)
      {
          if(Type[k]['province']==data)
          {
              temp=Type[k]['data']
              break;
          }
      }
      console.log(temp)
      this.drawCircle(temp)
    })
  },
  methods: {
   drawCircle(data){
    // 基于准备好的dom，初始化echarts实例
   var mydata=data
  // console.log(mydata)
    var bar_dv = this.$refs.circle;
    if (bar_dv){
        let myChart = this.$echarts.init(bar_dv)
        var option = {
             title:{
                text: this.location+'高校分类',
                left: "center",
                textStyle: {
                fontSize: 18,
                color: "black"//设置标题字体颜色
                 },
            },
        tooltip: {
            trigger: 'item'
    },
    legend: {
        bottom: '5%',
        width:'60%',
        orient:'horizontal',
        textStyle: { //图例文字的样式
                            color: 'black',
                            fontSize: 16
                        }
    },
    series: [
        {
            name: '访问来源',
            type: 'pie',
            radius: ['30%', '50%'],
            width:'40%',
            left:'30%',
            top:'-35%',
            avoidLabelOverlap: false,
            itemStyle: {
                borderRadius: 10,
                borderColor: '#fff',
                borderWidth: 2
            },
            label: {
                show: false,
                position: 'center'
            },
            color:['#0e8dcc','#29a8ee','#20c6dc','#02337f','#d59d4f','#f1d093','#cc890e','#e692a4','#be4084','#f1bcbd','#df95e','#a94748'],
            emphasis: {
                label: {
                    show: true,
                    fontSize: '40',
                    fontWeight: 'bold'
                }
            },
            labelLine: {
                show: false
            },
            data:mydata
        }
    ]
};
      myChart.setOption(option);
}
}}}
</script>

<style scoped>
.circle{
  width: 470px;
  height: 200px;
  margin-left: -25%;
  margin-top: 5%;
}
</style>
