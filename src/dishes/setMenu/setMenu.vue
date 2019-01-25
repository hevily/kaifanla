<template>
	<div class="system">
		<div class="setContainer">
			<div class="setName">
				<van-popup v-model="showName" position="top" >
				  	<van-cell-group>
					  <van-field
					    v-model="realName"
					    center
					    clearable
					    label="姓名"
					    placeholder="请输入真实姓名"
					  >
					   <van-button slot="button" @click="nextStepA" size="small" type="primary">下一步</van-button>
					  </van-field>
					</van-cell-group>
					</van-popup>
			</div>
			<div class="setMenu">
				<van-popup v-model="showSet" position="right" >
				  	<div class="weekMenu">
				  		<van-nav-bar
						  title="用餐情况"
						  left-arrow
  						  @click-left="onClickLeft"
						/>
						
						<div >
							<div >
								<van-checkbox v-model="weekRadio">其他天相同</van-checkbox>
							</div>
							
							<van-checkbox-group  style="display: flex; align-content: center; font-size:0.16rem" v-model="result"  v-for="(week,index) in weekList">
								 <div >周{{week.week}}</div>
								 <van-checkbox
								   	v-for="(meal,index) in week.meal"
								    :key="index"
								    :name="meal"
								  >
								    {{meal.eat}}
								 </van-checkbox>

							</van-checkbox-group>
						</div>
				  	</div>
				</van-popup>
			</div>
		</div>
		
	</div>
</template>

<script>
export default{
	data(){
		return{
			showName:true,
			showSet:false,
			weekRadio:true,
			realName:'',//真实姓名
			columns: [
		        { text: '杭州'	},
		        { text: '宁波' },
		        { text: '温州' }
	      	],
	      	weekList:[
	      		{ 
	      			week:"一",
	      			meal:[
	      				{eat:"午餐"},
	      				{eat:"晚餐"},
	      			]
	      		},
	      		{ 
	      			week:"二",
	      			meal:[
	      				{eat:"午餐"},
	      				{eat:"晚餐"},
	      			]
	      		},
	      		{ 
	      			week:"三",
	      			meal:[
	      				{eat:"午餐"},
	      				{eat:"晚餐"},
	      			]
	      		},
	      		
	      	],
	      	result: []
		}
	},
	methods:{
		nextStepA(){
			if( this.realName != '' ){
				this.showName=false;
				this.showSet=true;
			}
		},
		onClickLeft(){
			this.showName=true;
			this.showSet=false;
		},
		dd(i){
			console.log(i)
		}
	}
}
</script>

<style scoped>
	.setMenu .van-popup--right{
		width:100%;
		height:100%;
		background:white;
	}
	.van-checkbox{
		display: inline-block;
	}
</style>