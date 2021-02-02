<template>
  <div class="login" style="height:100%;min-width: 1000px;overflow:hidden;overflow-x:hidden;">
    <div class="head">
			<div class="left" style="position:relative;z-index:1000;cursor: pointer" @click="$router.push({name: 'home'})">
				<img :src="img1" alt="">
				<p>易动</p>
			</div>
			<div class="right">
				<!-- <span @click="$router.push({name: 'home'})">首页</span>
				<span>文档中心</span> -->
			</div>
		</div>
		<div class="bg">
		</div>
		<!-- <div class="content">
			<div class="left">
        <img :src="img2" class="left">
			</div>
			<div class="right">
				<p class="title">用户登录</p>
				<input v-model="username" type="text" placeholder="请输入邮箱账号/用户名">
				<input v-model="password" type="password" placeholder="请输入密码">
				<input type="text" class="input_other" placeholder="验证码">
				<img :src="code" alt="" clsss="code">
				<div>
					<el-checkbox v-model="checked" class="check">备选项</el-checkbox>
					<p class="forget">忘记密码</p>
				</div>
				<button @click="login">登录</button>
				<span class="block">没有账号？<span style="cursor:pointer;" @click="$router.push({name: 'register'})">立即注册</span></span>
			</div>
		</div> -->
		<div class="content">
			<div class="left">
				<img :src="img2" class="left">
			</div>
			<div class="right">
				<p class="title">用户登录</p>
				<div class="box">
					<el-input v-model="username" type="text" placeholder="请输入邮箱账号/用户名"></el-input>
				</div>
				<div class="box">
					<el-input v-model="password" type="password" placeholder="请输入密码"></el-input>
				</div>
				<div class="box" style="text-align:left;">
					<el-input v-model="code" type="text" placeholder="请输入验证码" style="width:65%;"></el-input>
					<img style="position:absolute;top:35px;left:70%;" :src="codeUrl" @click="getCode">
				</div>
				<div class="box">
					<el-checkbox v-model="rememberMe" class="check">保存密码</el-checkbox>
					<p class="forget" style="cursor:pointer;">忘记密码?</p>
				</div>
				<div class="box">
					<button @click="login" style="cursor:pointer;">登录</button>
					<br>
					<span class="block">没有账号？<span style="cursor:pointer;" @click="$router.push({name: 'register'})">立即注册</span></span>
				</div>
			</div>
		</div>
		<bottom style="position:relative;z-index:1000;"></bottom>
  </div>
</template>

<script>
import API from '@/api'
import bottom from '@/components/common/bottom'
export default {
  name: 'login',
  components: {
    bottom
  },
  data () {
    return {
      rememberMe: false,
      username: '',
      password: '',
      msg: '登录',
      img1: require('../../../assets/static/logo.png'),
      img2: require('../../../assets/static/login_person.png'),
      code: '',
      codeUrl: '',
      uniqueKey: ''
    }
  },
  created () {
    this.$store.state.public1 = false
  },
  mounted () {
    this.getCode()
  },
  methods: {
    login () {
      let oData = {
        usernameOrEmail: this.username,
        password: this.password,
        code: this.code,
        uniqueKey: this.uniqueKey,
        rememberMe: this.rememberMe
      }
      API.doLogin(oData).then(res => {
        if (res.data.code === 100) {
          if (res.data.result.token) {
            document.cookie = 'token=' + res.data.result.token
            this.$store.commit('setUsername', this.username)
            sessionStorage.setItem('loginState', 'login')
            sessionStorage.setItem('username', this.username)
			this.$router.push({name: 'home'})
            sessionStorage.setItem('menu', JSON.stringify(res.data.result.menuInfo))
          } else {
			  this.getCode()
		  }
        } else {
			this.getCode()
		}
      }).catch(err => {
		  this.getCode()
		  this.$message({type: 'error', message: err})
      })
    },
    getCode () {
      let oData = {}
      API.getCode(oData).then(res => {
		  console.log(res)
        if (res.data.code === 100) {
          this.codeUrl = 'data:image/jpeg;base64,' + res.data.result.encode
          this.uniqueKey = res.headers.uniquekey
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.head{
	color: #fff;
	height: 60px;
	border-bottom: rgba($color: #ddd, $alpha: 0.4) 1px solid;
	padding-left: 10%;
	padding-right: 10%;
	.left{
		float: left;
		margin-top: 10px;
		img{
			width: 45px;
		}
		p{
			float: right;
			font-size: 30px;
			margin-left: 10px;
		}
	}
	.right{
		float: right;
		span{
			line-height: 60px;
			margin-left: 40px;
			cursor: pointer;
		}
	}
}
.bg{
	position:fixed;
  top: 0;
  left: 0;
  width:100%;
  height:970px;
  min-width: 1000px;
  z-index:-10;
  zoom: 1;
  background-color: #fff;
  background-repeat: no-repeat;
  background-size: cover;
  -webkit-background-size: cover;
  -o-background-size: cover;
  background-position: center 0;
	background-image: url('../../../assets/static/login_bg.png');
}
.content{
	width: 100%;
	height: calc(100% - 132px);
	position: relative;
	.left{
		position: absolute;
		top: -30px;
	}
	.right{
		width: 350px;
		height: 500px;
		background-color: rgba($color: #0B2458, $alpha: 0.7);
		float: right;
		color: #fff;
		text-align: center;
		position: absolute;
		padding: 0 20px;
		right: 15%;
		top: 18%;
		.title{
			font-size: 28px;
			margin-top: 50px;
		}
		.box{
			padding-top: 30px;
			position: relative;
			.check{
				float:left;
				color:white;
			}
			button{
				width: 100%;
				height: 40px;
				border-radius: 4px;
				border: none;
				background-color: #007bff;
				font-size: 18px;
				color: #fff;
				margin-bottom: 20px;
			}
		}
	}
}
// .content{
// 	width: 100%;
// 	height: calc(100% - 132px);
// 	.left{
// 		width: 40%;
// 		height: 100%;
// 		float: left;
// 		margin-left: 50px;
// 		img{
// 			width: 100%;
// 		}
// 	}
// 	.right{
// 		width: 25%;
// 		height: calc(100% - 360px);
// 		background-color: rgba($color: #0B2458, $alpha: 0.7);
// 		float: right;
// 		margin-right: 10%;
// 		margin-top: 180px;
// 		color: #fff;
// 		text-align: center;
// 		.title{
// 			font-size: 24px;
// 			margin-top: 42px;
// 		}
// 		input{
// 			width: 90%;
// 			height: 40px;
// 			margin-top: 25px;
// 			border-radius: 4px;
// 			border: none;
// 			float: left;
// 			margin-left: 5%;
// 			padding-left: 5px;
// 			font-size: 16px;
// 		}
// 		::-webkit-input-placeholder{
// 				color: #aaa;
// 				font-size: 16px;
// 		}
// 		.input_other{
// 			width: 60%;
// 		}
// 		div{
// 			height: 60px;
// 			line-height: 60px;
// 			width: 90%;
// 			margin-left: 5%;
// 			font-size: 13px;
// 			.check{
// 				float: left;
// 				color: #fff;
// 			}
// 			.forget{
// 				float: right;
// 				cursor: pointer;
// 			}
// 		}
// 		button{
// 			width: 90%;
// 			height: 40px;
// 			border-radius: 4px;
// 			border: none;
// 			background-color: #007bff;
// 			font-size: 18px;
// 			color: #fff;
// 		}
// 		.block{
// 			display: block;
// 			float: right;
// 			margin-right: 5%;
// 			margin-top: 30px;
// 			font-size: 13px;
// 			span{
// 				color: #007bff;
// 			}
// 		}
// 	}
// }
// .footer{
// 	color: #BDBEC2;
// 	font-size: 12px;
// 	width: 100%;
// 	height: 70px;
// 	background-color: #0B2458;
// 	.left{
// 		float: left;
// 		margin-left: 10%;
// 		p{
// 			margin-top: 10px;
// 		}
// 	}
// 	.right{
// 		float: right;
// 		margin-right: 10%;
// 		margin-top: 10px;
// 		span{
// 			margin-right: 20px;
// 			cursor: pointer;
// 		}
// 	}
// }
</style>
