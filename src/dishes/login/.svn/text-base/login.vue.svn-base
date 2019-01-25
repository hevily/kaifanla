<template>
	<div class='login'>
		<div class='bg'></div>
		<div class="loginBox" v-show="loginShow">
			<van-cell-group>
			  <van-field
			    v-model="userName"
			    clearable
			    label="手机"
			    placeholder="请输入用户名"
			    @click-icon="$toast('question')"
			  />

			  <van-field
			    v-model="userPwd"
			    type="password"
			    label="密码"
			    placeholder="请输入密码"
			  />
			</van-cell-group>
      <div class="regis" @click="goRegister">去注册</div>
      <div class="btnBox">
        <van-button size="large" @click="goGoA" class="btn" type="default">登录</van-button>
      </div>
		</div>

    <div class="registerBox" v-show="registerShow">
      <van-cell-group>
        <van-field
          v-model="resUserName"
          clearable
          label="手机"
          placeholder="请输入账号"
          @click-icon="$toast('question')"
        />

        <van-field
          v-model="resUserPwd"
          type="password"
          label="密码"
          placeholder="请输入密码"
        />
        <van-field
          v-model="resRePwd"
          type="password"
          label="确认密码"
          placeholder="请再次输入密码"
        />
      </van-cell-group>
      <div class="regis" @click="goLogin">去登录</div>
      <div class="btnBox">
        <van-button size="large" @click="goGoB" class="btn" type="default">注册</van-button>
      </div>
    </div>
	</div>
</template>

<script>
  import { Toast } from 'vant';
export default{
	data(){
		return{
			userName:'',
			userPwd:'',
      resUserName:'',
      resUserPwd:'',
      resRePwd:'',
      loginShow:true,
      registerShow:false,
		}
	},
	mounted() {
    // Toast.success('asdf')

    },
	methods:{
		goGoA(){
		  let that = this
      let qs = require('qs')
		  let lists = {}
      lists.mobile = that.userName
      lists.password = that.userPwd
      that.$fetch('login', qs.stringify(lists)).then(rs => {
        that.$store.commit('login', rs);
        Toast.loading({
          mask: true,
          message: '登录中...',
          duration:1000
        });
        setTimeout(()=>{
          that.$router.push({
            path: '/',
          })
        },1200)
      })
		},
    goGoB(){

      let that = this
      let qs = require('qs')
      let lists = {}
      lists.mobile = that.resUserName
      lists.password = that.resUserPwd
      lists.confirm_password = that.resRePwd
      that.$fetch('register', qs.stringify(lists)).then(rs => {
        that.$store.commit('login', rs);
        Toast.loading({
          mask: true,
          message: '请稍候...',
          duration:1000
        });
        setTimeout(()=>{
          that.$router.push({
            path: '/',
          })
        },1200)
      })
    },
    goRegister(){
      this.loginShow=false
      this.registerShow=true
    },
    goLogin(){
      this.loginShow=true
      this.registerShow=false
    },
	}
}
</script>

<style scoped>
	.bg{
		width:100%;
		height:2.35rem;
		background-image:url('../../../static/images/topBg.png');
		background-size:100% 100%;
		margin-bottom: 0.5rem;
	}
	.loginBox,.registerBox{
		width:80%;
		margin:0 auto;
		font-size:0.16rem;
	}
	.btnBox{
		width:100%;
		margin:0.5rem auto;
	}
  .regis{
    margin-top:0.1rem;
    margin-left:0.15rem;
    font-size:0.14rem;
    color:#ff9000;
    cursor: pointer;
  }
	.btn{
		background:#ff9000;
		color:white;
		font-size:0.14rem;
		border-radius: 0.4rem;
	}
	.van-button--large{
		height:42px;
		line-height:42px;
	}
	[class*=van-hairline]::after{
		border:none;
	}
</style>
