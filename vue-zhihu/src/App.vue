<template>
<el-row>
<el-col :span="3">
      <el-menu text-color="#687788" @select="handleSelect" :default-active="activeIndex" router>
      <el-menu-item index="/">每日推荐</el-menu-item>
      <el-menu-item index="2">主题日报</el-menu-item>
      </el-menu>
</el-col>
<el-col :span="5" v-if="activeIndex=='/'">
<h5>{{news.date}}</h5>
    <el-menu router text-color="#687788">
      <el-menu-item v-for="(item,index) in news.stories" :index="'1'+index" :route="'/main/'+item.id" :key="item.id" class="item">
        <template>
          <div class="item-div">
          <img :src="'/static/imgs/'+item.images" width="70px" style="border-radius:5px;margin-right:5px">
          <p>
          {{item.title}}
          </p>
          </div>
        </template>
      </el-menu-item>
      </el-menu>
</el-col>
<el-col :span="16" v-if="activeIndex=='/'">
    <router-view/>
</el-col>
</el-row>



</template>

<script>
export default {
data(){
  return{
    activeIndex:'',
    news:{},
    day:''
  }
},
beforeMount(){
  this.$axios.get('/static/news.json').then((result) => {
	console.log(result.data)
	this.news = result.data
  }).catch((err) => {
    console.error(err)
  });
},
methods:{
 handleSelect(key, keyPath) {
        this.activeIndex = key
      }
}
}
</script>

<style>
body{
  margin: 0%;
}
.el-row{
  text-align: center;
  color: #687788; 
}
.el-col-16{
  background-color:#F4F6F8
}
.item{
  text-align: left;
  height: 80px;
}
.item-div{
  height: 80px;
  display: flex;
  flex-flow: row nowrap;
  align-items: center;
}
.item-div p{
  display: inline-block;
  width: 100%;
  white-space: initial;
  line-height: 140%;
}
</style>

