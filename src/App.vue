<template>
   <div class="app">
       <div class="border-box"><p>高考志愿填报推荐可视分析系统</p></div>
             <div class="myContainer">
                 <div style="width:20%; height:100%; float:left; display:inline">
                     <my-select-menu class= "box" style="height:50%;"></my-select-menu>
                     <!-- <mycircle class= "box" style="height:50%;"></mycircle> -->
                    <my-list class= "box" style="height:50%"></my-list>
                 </div>
                <!-- <my-select-menu class= "box" style="width:20%; height:50%; float:left; display:inline"></my-select-menu>
                <mycircle2 class= "box" style="width:25%;  height:auto; float:left; display:inline"></mycircle2> -->
                <mymap class= "box" style="width:57%;  height:100%; float:left; display:inline;"></mymap>
                <div class= "box" style="width:23%;  height:120%; float:left; display:inline">
                <enter-score ></enter-score>
                <my-line-chart ></my-line-chart>
                </div>
           </div>
           <div class="foot">
                <mycircle class= "box" style="width:20%;  height:100%; float:left; display:inline"></mycircle>
               <!-- <my-list class= "box" style="width:20%;  height:100%; float:left; display:inline"></my-list> -->
               <!-- <mycircle class= "box" style="width:28%; height:auto; float:left; display:inline"></mycircle>
                <mycircle2 class= "box" style="width:25%;  height:auto; float:left; display:inline"></mycircle2> -->
                <school-para  class= "box" style="width:57%;  height:100%; float:left; display:inline"></school-para>
                <my-world-cloud  class= "box" style="width:23%;  height:100%; float:left; display:inline"></my-world-cloud>  
           </div>
      </div>

</template>

<script>
import mymap from './components/map';
import enterScore from './components/enterScore';
import schoolPara from './components/schoolPare';
import mycircle from './components/mycircle';
import mycircle2 from './components/mycircle2';
import myList from './components/myList';
import myWorldCloud from './components/worldCloud';
import myLineChart from './components/lineChart';
import mySelectMenu from './components/selectMenu';
import PubSub from 'pubsub-js'
export default {
  name: 'App',
  components:{
    mymap,
    enterScore,
    schoolPara,
    //schoolpara_2 ,
    mycircle,
    mycircle2,
    myList,
    myWorldCloud,
    myLineChart,
    mySelectMenu
    },
    data: function () {
      return {
          viewChange:true
      }
    },
    mounted(){
    PubSub.subscribe("clickSchool", (msg, data) => {
        this.viewChange=false
        console.log("false",this.viewChange)
    });
    PubSub.subscribe("pub_mapProvince", (msg, data) => {
        this.viewChange=true
    });
    },
}
</script>

  <style>
        /* 三栏布局，中间自适应高度... */
        * {
            margin: 0;
            padding: 0;
        }    
        /* .head{
            height: 100px;
            width: 100%;
            background-color: gold;
            text-align: center;
        } */
        .app{
           background-color: "white";
           
        }
        .myContainer{
            position: absolute;
            width: 100%;
            height: 70%;
            top: 37px;
            background-color: "white";
            text-align: center;
            background-color: white;
        }
        /* .left{
            width: 30%;
            background-color: #145b7d;
            text-align: center;
        }
        .right{
            width: 34%;
            background-color: #145b7d;
            text-align: center;
        }
        .mid{
            width: 50%;
            flex: 1;
            text-align: center;
        } */
        .foot{
            position: absolute;
            width: 100%;
            height: 30%;
            bottom: 0;
            background-color: "white";
            text-align: center;
        }
        .box{
            box-shadow: 0px 0px 20px #333399;
            border-style:outset;
            background-color: "white";
        }
        .border-box{
            position: relative;
            margin:4px auto;
            /* width:400px; */
            height:30px;
            background: "white";
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
            color:black;
        }

</style>
