<template>
<el-row>
	<el-col :span="12" :offset="6" style="text-align:left">
		<div class="tit" style="text-align:center">
			<img :src="'/static/imgs/'+News.images" width="90%" height="340px">
			<h2 style="text-align:left">{{News['question-title']}}</h2>
		</div>
		<div v-if="News.questions">
			<div v-for="(item,index) in News.questions" :key="index" :style="{'border-bottom':'2px solid #ccc','margin-bottom':'10px'}">
			<h4 v-if="item.title">{{item.title}}</h4>
			<p><img :style="text" :src="'/static/imgs/'+item['question-title-src']" v-if="item['question-title-src']">{{item.author}}<span :style="span">{{item.bio}}</span> </p>
			<p v-html="item.content"></p>
			<p><img :src="'/static/imgs/'+item['content-image']" width="100%" v-if="item['content-image']"></p>
		</div>
		</div>
		<div v-if="!News.questions">
		<div>
			<h4 v-if="News.title">{{News.title}}</h4>
			<p><img :src="'/static/imgs/'+News['question-title-src']" :style="text" v-if="News['question-title-src']">{{News.author}}<span :style="span">{{News.bio}}</span> </p>
			<p v-html="News.content"></p>
			<p><img :src="'/static/imgs/'+News['content-image']" width="100%" v-if="News['content-image']"></p>
		</div>
		<div v-if="News.schema">
			<hr>
			<p v-for="item in News.schema" :key="item">{{item}}</p>
		</div>
		<hr>
		</div>
		<el-button @click="hid = (hid == true)?false:true" style="width:100%">查看知乎讨论</el-button>
		<div class="comment" v-if="hid">
			<h4>评论({{Comments.comments.length}})</h4>
			<ul>
				<li v-for="(com,index) in Comments.comments" :key="index">
					<div>
						<p >
					<img :src="'/static/imgs/'+com.avatar" width="50px" :style="text"><span :style="span">{{com.author}}</span>
						</p>
						<p style="font-size:13px">{{getTm(com.time)}}</p>
						<p>{{com.content}}</p>
					</div>
				</li>
			</ul>
			
		</div>
	</el-col>
</el-row>
</template>

<script>
export default {
	data(){
		return{
			News:{},
			itemId:'',
			Comments:{},
			hid:false,
			date:'',
			text:{
				'vertical-align': 'middle'
			},
			span:{
				'font-size':'13px',
				'color':'#000'
			}
		}
	},
	beforeMount(){
		let dt = new Date()
		this.date = dt.getTime()
		this.getMes();
	console.log(1,this.News.questions)
		},
watch:{
	$route(){
		let dt = new Date()
		this.date = dt.getTime()
		this.getMes();
	},
},
methods:{
	getMes(){
		this.itemId = this.$route.params.id;
		this.$axios.get(`/static/${this.itemId}-news.json`).then((resp) => {
			this.News = resp.data;
		console.log(this.News)
		}).catch((err) => {
			console.error(err)
		});
		this.$axios.get(`/static/${this.itemId}-comment.json`).then((resp) => {
			console.log(resp.data)
			this.Comments = resp.data;
		}).catch((err) => {
			console.error(err)
		});
	},
	getTm(t){
		let tim = this.date/1000 - t;
		if(tim/60 <= 24){
			return parseInt(tim/60)+'分钟前'
		}else{
			return parseInt(tim/60/24)+'小时前'
		}
	}
}
}
</script>

<style>
.el-col-12{
background-color: #fff;
}
.el-col-12 div{
	padding: 0px 5px;
}
.tit h2{
	position: absolute;	
	width:35%;
	white-space: initial;
	color: #ffffff;
	top: 200px;
	left: 335px;
}
.comment ul{
	padding: 0;
}
.comment ul li{
	list-style: none;
	margin: 5px 0;
	border-bottom: 1px solid #CCC;
}

</style>
