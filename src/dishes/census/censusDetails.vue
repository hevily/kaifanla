<template>
  <div class="censusDetails">
    <van-nav-bar
      title="用餐明细"
      left-arrow
      @click-left="onClickLeft"
    />
    <div class="body">
      <div class="dateList">
        <div class="date" v-for="(item,index) in dateList">
          <div class="left">
            <div>{{index}}</div>
            <div style="color: #18b4ed;">{{item.day}}</div>
          </div>
          <div class="right">
            <div :class="{checked : item.meal.lunch == 1 && checked}" @click="setMenu(index, 'lunch', item.meal.lunch == 1)">午餐</div>
            <div :class="{checked : item.meal.dinner == 1 && checked}" @click="setMenu(index, 'dinner', item.meal.dinner == 1)">晚餐</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "censusDetails",
    data () {
      return {
        dateList: [],
        checked: 1,
        order_id: 0
      }
    },
    created(){
      if(!this.$route.query.data){
        this.$fetch('order_meals',this.$qs.stringify({member_id: this.$store.state.user_id})).then(rs => {
          this.dateList = rs.month_meals;
          this.order_id = rs.order_id;
        })
      }else{
        this.dateList = this.$route.query.data.month_meals
        this.order_id = this.$route.query.data.order_id;
      }
    },
    methods: {
      onClickLeft () {
        this.$router.back(-1)
      },
      setMenu (index, type, state) {
        this.$dialog.confirm({
          title: '更改用餐',
          message: '是否' + (!state ? '选择' : '取消') + (Number(index)) + '日' + (type == 'lunch' ? '午餐' : '晚餐')
        }).then(() => {
          let req = {};
          req.member_id = this.$store.state.user_id;
          req.order_id = this.order_id;
          req.day = index;
          req.meal = {};
          Object.keys(this.dateList[index]['meal']).forEach(key => {
            req.meal[key] = this.dateList[index]['meal'][key];
          })
          if(req.meal[type] == 0){
            req.meal[type] = 1
          } else {
            req.meal[type] = 0
          }
          this.$fetch('order_change',this.$qs.stringify(req)).then(rs => {
            let val = this.dateList[index]['meal'][type];
            val == 0 ? this.dateList[index]['meal'][type] = 1 : this.dateList[index]['meal'][type] = 0
            this.checked++//刷新dom
          })
        }).catch(() => {
          // on cancel
        });
      }
    }
  }
</script>

<style lang="less" scoped>
  .censusDetails{
    .body{
      padding: .1rem;
      padding-right: 0;
      .dateList{
        display: flex;
        flex-wrap: wrap;
        .date{
          display: flex;
          align-items: center;
          justify-content: space-between;
          border: 1px solid #000000;
          width: calc(20% - .1rem - 2px);
          margin: 0 .1rem .1rem 0;
          //margin: 0 .1rem .1rem 0;
          //padding: .05rem 0;
          .left{
            width: .3rem;
            text-align: center;
          }
          .right{
            >div{
              border-radius: .05rem 0 0 .05rem;
              border: 1px solid #000000;
              border-right: 0;
              margin: .05rem 0;
              font-size: .12rem;
            }
            .checked{
              background: #18b4ed;
              color: #fff;
            }
          }
        }

        .date:before {
          content: "";
          display: inline-block;
          padding-bottom: 100%;
          width: .1px;
          vertical-align: middle;
        }
      }
    }
  }
</style>
