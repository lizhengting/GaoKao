<template >
  <div class="mylist" id='div-container' >
    <ul class="list-group" style="color:black" id='container' >
    </ul>
  </div>
</template>


<script>
import Type from '../../static/学校层次.json'
import '../../node_modules/bootstrap/dist/css/bootstrap.min.css';
import '../../node_modules/bootstrap/dist/js/bootstrap.min.js';
import PubSub from 'pubsub-js';
import $ from 'jquery'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted(){
    this.drawList()
    PubSub.subscribe("onClickBtn", (msg, data) => {
      var schoolInfo=data.schoolInfo;
      var scoreInfo=data.scoreInfo
      this.upDateList(schoolInfo,scoreInfo)
    });
    },
    methods:{
        drawList()
        {
          var container=document.getElementById('container')
          for(let i=0;i<Type.length;i++)
          {
            var li=document.createElement("li");
            li.setAttribute("class","list-group-item");
            
            if(Type[i]['办学层次']=='985')
            {
              var span=document.createElement("span")
              span.setAttribute("class","badge badge-success")
              span.innerHTML+='985'
              li.appendChild(span)
            }
            if(Type[i]['办学层次']=='211')
            {
              var span=document.createElement("span")
              span.setAttribute("class","badge badge-warning")
              span.innerHTML+='211'
              li.appendChild(span)
            }
            
            li.innerHTML+=Type[i]['学校名称']
            li.onclick=function(){
              var str
              str=this.innerHTML;
              str=str.split('>')
              PubSub.publish("clickSchool",str[str.length-1])
            }

            container.appendChild(li)
            
          }
        },

        upDateList(schoolList,score){
          var ul=document.getElementById('container')
          $("#container").empty()
          for(let i=0;i<schoolList.length;i++)
          {
            var li=document.createElement('li')
            li.setAttribute("class","list-group-item");
            if(schoolList[i]['分数']>score+10)
            {
              var span=document.createElement("span")
              span.setAttribute("class","badge badge-warning")
              span.innerHTML+='冲'
              li.appendChild(span)
            }
            else if(schoolList[i]['分数']<score-10)
            {
              var span=document.createElement("span")
              span.setAttribute("class","badge badge-success")
              span.innerHTML+='保'
              li.appendChild(span)
            }
            else{
              var span=document.createElement("span")
              span.setAttribute("class","badge badge-info")
              span.innerHTML+='稳'
              li.appendChild(span)
            }

            li.innerHTML+=schoolList[i]['学校']
            li.onclick=function(){
              var str
              str=this.innerHTML;
              str=str.split('>')
              PubSub.publish("clickSchool",str[str.length-1])
            }

            ul.appendChild(li)
          }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.mylist{
  width: 100%;
  background-color:white;
}
.list-group{
width: 100%;
max-height: 225px;
overflow-y:scroll; 
overflow-x:hidden;
}
.list-group-item{
flex: none;
}
li{
  height: 50px;
  color: white;
  
}
</style>
