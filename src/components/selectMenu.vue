<template>
  <div class="selectMenu" >
    <div class="Menu" id='score'>
       <span>分数：</span>
      <el-input id="scoreInput" v-model="input" :rows="3" size='mini' style="width:110px;margin-left:20px" placeholder="请输入内容"></el-input>
    </div>
    <div  id="province" class="Menu">
      <span>意向省市：</span>
      <el-select v-model="provinceValue" size='mini' style="width:110px" placeholder="请选择">
        <el-option
          v-for="item in provinceOptions"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
    </div>
    <div class="Menu" id="type">
      <span>院校类型：</span>
      <el-select v-model="typeValue" size='mini' style="width:110px" placeholder="请选择">
        <el-option
          v-for="item in classOptions"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
    </div>
    <div class="Menu" id="level">
      <span>院校等级：</span>
      <el-select v-model="levelValue" size='mini' style="width:110px" placeholder="请选择">
        <el-option
          v-for="item in levelOptions"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
    </div>
    <div class="Menu" id='score'>
      <span style="margin-left:-38%">分科选择：</span>
      <el-radio v-model="radio" style="color:#ccc" label="理科">理科</el-radio>
      <el-radio v-model="radio" style="color:#ccc" label="文科">文科</el-radio>
    </div>
     <el-button type="primary" round @click="btnClick">点击推荐</el-button>
  </div>
</template>


<script>
// import '../../node_modules/bootstrap/dist/css/bootstrap.min.css';
// import '../../node_modules/bootstrap/dist/js/bootstrap.min.js';
import stataic from '../../static/余弦相似度.json'
import PubSub from 'pubsub-js';
export default {
  name: 'SelecteMenu',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      provinceOptions: [],
      levelOptions:[],
      classOptions:[],
      input:'',
      radio:'理科',
      value:'',
      provinceValue:'',
      typeValue:'',
      levelValue:''
    }
  },
  mounted(){
    //初始化省市选择下拉框
          var province=['黑龙江','吉林','辽宁','上海','江苏','浙江','安徽','福建','江西','山东','北京','天津','山西','河北','内蒙古','河南','湖北','湖南','广东','广西','海南','重庆','四川','贵州','云南','西藏','陕西','甘肃','青海','宁夏','新疆']
          var proData=[]
          for(let i = 0; i < province.length;i++)
          {
            let val=province[i]
            let obj={
              value:val,
              label:province[i]
            }
            proData.push(obj)
          }
        this.provinceOptions=proData
    //初始化学校等级
        var  schoolLevel=['985','211','本科','专科']
          var levelData=[]
          for(let i = 0; i < schoolLevel.length;i++)
          {
            let val=schoolLevel[i]
            let obj={
              value:val,
              label:schoolLevel[i]
            }
            levelData.push(obj)
          }
        this.levelOptions=levelData
      
    //初始化学校分类
       var schoolClass=['综合','财经','工科','艺术','师范','体育','政法','农业','医药','民族','语言','林业']
       var classData=[]
          for(let i = 0; i < schoolClass.length;i++)
          {
            let val=schoolClass[i]
            let obj={
              value:val,
              label:schoolClass[i]
            }
            classData.push(obj)
          }
        this.classOptions=classData
    },
    methods:{
        btnClick(){
          var myLevel=['专科','本科','211','985']
          var myType=['综合','工科','师范','农业','林业','政法','医药','财经','民族','语言','艺术','体育']
          var myProvince=['黑龙江','吉林','辽宁','上海','江苏','浙江','安徽','福建','江西','山东','北京','天津','山西','河北','内蒙古','河南','湖北','湖南','广东','广西','海南','重庆','四川','贵州','云南','西藏','陕西','甘肃','青海','宁夏','新疆']
          var userInput=[]
          userInput[0]=0
          if(this.input==''){
            alert('请输入分数！')
            return
          }
          else{
            var content=this.input
            content=parseInt(content)
            if(content>750||content<0)
            {
              alert('请输入正确的分数！')
              return
            }
            else{
              //用户输入学校等级
              for(let i=0;i<myLevel.length;i++)
              {
                if(this.levelValue==myLevel[i])
                {
                  userInput.push(i+1)
                  break;
                }
              }
              //用户输入学校类型
              for(let i=0;i<myType.length;i++){
                if(this.typeValue==myType[i])
                {
                  userInput.push(i+1)
                  break;
                }
              }
              //用户输入学校地点
              for(let i=0;i<myProvince.length;i++)
              {
                if(this.provinceValue==myProvince[i])
                {
                  userInput.push(i)
                  break;
                }
              }
              userInput.push(content)
            }
          }

          
          this.cosSimilarity(userInput,this.radio)
          console.log(userInput,this.radio)
        },

      cosSimilarity(data,analogy){
        var analogyData=stataic[analogy]

        var jieguo=[]
        let num=0;
        for(let m=1;m<data.length;m++)
        {
            num=data[m]*data[m]+num
        }
        num=Math.sqrt(num)

        for(let i=0;i<analogyData.length;i++)
        {
            let x=0;
            let num2=0;
            for(let j=1;j<analogyData[i].length;j++)
            {
                x=x+data[j]*analogyData[i][j]
                num2=analogyData[i][j]*analogyData[i][j]+num2
            }
            num2=Math.sqrt(num2)

            let out=x/(num*num2)
            jieguo.push(out)

        }
        // console.log(jieguo)

        //排序
        let orin=jieguo.concat()

        jieguo.sort(function(a, b) {
            return a - b;
          });
        jieguo.reverse()
        // console.log('结果',jieguo)
        // console.log('orin',orin)
        let myIndex=[]
        for(let i=0;i<jieguo.length;i++)
        {
            myIndex.push(orin.indexOf(jieguo[i]))
        }
        // console.log(myIndex)
        var school=[]
        var flag=0
        var same=0
        var scor=data[4]
        for(let i=0;i<jieguo.length;i++)
        {
            if(flag>=10) break;
            else{
                var n=analogyData[myIndex[i]][0]
                if(analogyData[myIndex[i]][4]>(scor-30)&&(analogyData[myIndex[i]][4]<(scor+30)))
                {
                    var obj={
                        学校:n,
                        分数:analogyData[myIndex[i]][4]
                    }
                    school.push(obj)
                    flag++
                    if(n==same)
                    {
                      flag--
                    }
                    else{
                      same=n
                    }
                }
                
            }
        }
        // console.log('obj',school)
        var convData={
          schoolInfo:school,
          scoreInfo:data[4]
        }
        PubSub.publish("onClickBtn", convData)
      }


    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.selectMenu{
    width: 100%;
}
.Menu{
    margin: 5px 0 4% 25% ;
    width: 145px;
    float: left;
    width: 70%;
}
span{
  color: black;
  width: 50%;
  margin-left:-35%;
}
button{
  margin-top: -25px;
}
#score span{
  margin-left:-30%;
}
#score div{
  margin-left: 6%;
}
</style>
