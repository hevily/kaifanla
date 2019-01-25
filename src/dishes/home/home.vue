<<<<<<< .mine
<template>
	<div class="home">
		<div>
			<van-tabbar v-model="active" :fixed="false">
			  <van-tabbar-item >一</van-tabbar-item>
			  <van-tabbar-item >二</van-tabbar-item>
			  <van-tabbar-item >三</van-tabbar-item>
			  <van-tabbar-item >四</van-tabbar-item>
			  <van-tabbar-item >五</van-tabbar-item>
			  <van-tabbar-item >六</van-tabbar-item>
			</van-tabbar>
			<div class="order">
				<van-panel title="午餐" desc="10人" status="我已点餐"></van-panel>
				<van-panel title="晚餐" desc="10人" status="我已点餐"></van-panel>
			</div>
			<div class="rankContainer" style="margin-top:0.2rem">
				<div style="margin-left:0.1rem">菜式排名</div>
				<van-row 
					v-for="(item,index) in menuList"
					type="flex" 
					justify="center" 
					style="display: flex; align-items: center; justify-content: center;"
				>
				  <van-col span="6" style="height:0.5rem; line-height:0.5rem; text-align: center;">
				  	<span style="font-size:0.12rem">{{item.eat}}</span> 
				  	<van-tag>素</van-tag>
				  </van-col>
				  <van-col span="12" style="text-align: center;">
				  	<van-progress
					  color="#f44"
					  :percentage="item.pro"
					/>
				  </van-col>
				  <van-col span="6" style="text-align: center;">
				  	{{item.num}}人喜欢
				  </van-col>
				</van-row>
			</div>
		</div>
		
	</div>
</template>

<script>
export default{
	data(){
		return{
			active: 0,
			menuList:[
				{
					eat:"鱼香茄子",
					pro:50,
					num:8,
				},
				{
					eat:"鱼香茄子",
					pro:50,
					num:8,
				},
				{
					eat:"鱼香茄子",
					pro:50,
					num:8,
				},
			]
		}
	}
}
</script>

<style scoped>
	.order{
		
	}
	.van-tag{
		color:green;
		background:none;
	}
	
</style>
<style>
	.home .van-tabbar-item__text{
		font-size:0.14rem !important;
	}
</style>=======
<template>
  <div class="home">
    <div class="box">
      <div class="onul">
        <ul class="ul">
          <div class="line"></div>
          <li class="li" :id="'li'+item.date" v-for="(item,index) in dateList" @click="list_num(item)">
            <div class="day">{{item.day}}</div>
            <div class="round" :class="{check : index === num -1}"></div>
            <div class="date" :class="{uncheck : index !== num -1}">{{item.date}}</div>
          </li>
        </ul>
        <div class="text">
          <div class="wu">
            <div class="left">午餐</div>
            <div class="center">{{lunch_number}}</div>
            <div class="right">
              <div>人已点餐</div>
							<div v-if="my_lunch == 1" class="span_d">我已点餐</div>
              <div v-else class="span_w">我未点餐</div>
            </div>
          </div>
          <div class="wan">
            <div class="left">晚餐</div>
            <div class="center">{{dinner_nubmer}}</div>
            <div class="right">
              <div>人已点餐</div>
              <div v-if="my_dinner == 1" class="span_d">我已点餐</div>
              <div v-else class="span_w">我未点餐</div>
            </div>
          </div>
        </div>
      </div>
      <!--喜欢菜式-->
      <div class="box_cai">
        <div class="box_cai_title">
          喜好菜式
        </div>
        <ul>
          <li class="box_cai_li" v-for="item in cai_list">
            <div class="box_cai_text">
              {{item.dish_name}}
            </div>
            <div class="box_cai_hua">
              <van-slider v-model="item.favorite_percent" bar-height="0.05rem" disabled />
            </div>
            <div class="box_cai_text">
              {{item.favorite}}个人表示喜欢
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import utils from '@/utils/utils'
  export default {
    data() {
      return {
        list: ['日', '一', '二', '三', '四', '五', '六'],
        dateList: [],
        num: new Date().getDate(),
        lunch_number:0,
        dinner_nubmer:0,
        my_lunch:false,
        my_dinner:false,
        cai_list:[]
      }
    },
    created () {
      this.getDateList()
    },
    mounted() {
      this.list_num()
      this.food()
      if(this.num > 4 && this.num < 27){
        let num = this.num + 3;
        num > 28 ? num = this.num : 1
        this.$el.querySelector('#li' + num).scrollIntoView();
      }else{
        this.$el.querySelector('#li' + this.num).scrollIntoView();
      }

    },
    methods: {
      getFormatDay(time){
        let data = new Date(time)
        if(data){
          let day = data.getDay()
          let cnDay = ''
          switch (day){
            case 0 :
              cnDay = '日'
              break
            case 1 :
              cnDay = '一'
              break
            case 2 :
              cnDay = '二'
              break
            case 3 :
              cnDay = '三'
              break
            case 4 :
              cnDay = '四'
              break
            case 5:
              cnDay = '五'
              break;
            case 6 :
              cnDay = '六'
              break
          }
          return cnDay
        }else{
          console.log('时间格式有误：' + time)
          return ''
        }
      },
      getDateLength () {
        var date = new Date();
        var year = date.getFullYear()
        var month = date.getMonth()+1
        var d = new Date(year, month, 0)
        return d.getDate()
      },
      getDateList () {
        let _this = this
        for (var i = 0; i< this.getDateLength(); i++) {
          let date=new Date()
          date.setDate(i+1)
          let data = {}
          data.formatDate = utils.format(date)
          data.date = date.getDate()
          data.day = this.getFormatDay(date)
          _this.dateList.push(data)
        }
      },
      onClickLeft () {
        this.$router.push({
          path: '/dishes_memder'
        })
      },
      food (){
        this.$fetch('food', this.$qs.stringify({rows:20})).then(rs => {
          this.cai_list=rs.list
        })
      },
      list_num (item){
        let req = {}
        req.member_id = this.$store.state.user_id
        if (item) {
          this.num = item.date
          req.date = item.formatDate
        }
        this.$fetch('order_count',this.$qs.stringify(req)).then(rs => {
          this.lunch_number = rs.lunch_number
          this.dinner_nubmer = rs.dinner_nubmer
          this.my_lunch = rs.my_lunch
          this.my_dinner = rs.my_dinner
        })
      },
    },
  }
</script>

<style scoped lang="less">
  .home {
    height: 100%;
    width: 100%;
    background: url("../../../static/images/home_bgi.jpg") no-repeat;
    background-size: 100% 100%;
    overflow-y: auto;
    .box {
      padding: 0 .15rem;
      .onul {
        margin-bottom: .1rem;
        .ul {
          display: flex;
          justify-content: space-between;
          position: relative;
          overflow-x: auto;
          -webkit-overflow-scrolling: touch;
          .li {
            display: flex;
            flex: 0 0 14%;
            flex-direction: column;
            align-items: center;
            line-height: .44rem;
            margin-bottom: .1rem;
            text-align: center;
            font-size: .18rem;
            color: #18b4ed;
            .round{
              width: .1rem;
              height: .1rem;
              background: #999;
              border-radius: 50%;
            }
            .round.check{
              background: #18b4ed;
              transform: scale(1.4);
            }
            .uncheck {
              color: #999;
            }
          }
          .line{
            position: absolute;
            width: 430%;
            height: 1px;
            background: #999;
            position: absolute;
            top: 50%;
            margin-top: -0.05rem;
          }
        }
        .text{
          .wan , .wu{
            font-size: .16rem;
            display: flex;
            align-items: center;
            color: #18b4ed;
            .left{
              font-size: .4rem;
            }
            .center{
              font-size: .8rem;
              padding: 0 .2rem;
            }
            .right{
              .span_d{
                color: #18b4ed;
              }
              .span_w{
                color: #ff1d92;
              }
            }
          }
        }
      }
      .box_cai{
        overflow: auto;
        .box_cai_title{
          color: #18b4ed;
          font-size: .26rem;
        }
        .box_cai_li{
          display: flex;
          padding: 0 0.12rem;
          height: .44rem;
          line-height: .44rem;
          align-items: center;
          font-size: .12rem;
          justify-content: space-between;
          .box_cai_text{
            flex: 0 0 25%;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
          .box_cai_hua{
            margin-right: 1%;
            flex: 0 0 48%;
          }
        }
      }
    }

  }
</style>
<style type="text/css">

</style>
>>>>>>> .r18259
