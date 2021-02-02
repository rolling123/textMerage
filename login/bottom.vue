<!--home模块中最下面关于公司的一些东西-->
<template>
    <section>
        <div class="bg">
			<div class="left">
				<p>Copyright @ 2003-2019 浙江浙大中控信息技术有限公司  保留一切权利  浙IPC备05002928号</p>
				<p>浙公安网安备33010802002206</p>
				dddddddddddddddddddd
			</div>
			<div class="right">
				<span>法律声明</span>
				<span>隐私政策声明</span>
				<span>可接受的使用政策</span>
				<span>移动应用用户协议</span>
				<span>举报不良信息</span>
			</div>
		</div>
    </section>
</template>

<script>
export default {
  name: 'homeBottom',
  data () {
    return {

    }
  }
}
</script>

<style lang="scss" scoped>
.bg{
	color: #BDBEC2;
	font-size: 12px;
	width: 100%;
	height: 70px;
	background-color: rgb(36,46,66);
	.left{
		float: left;
		margin-left: 10%;
		p{
			margin-top: 10px;
		}
	}
	.right{
		position: absolute;
		bottom: 5px;
		right: 0px;
		margin-right: 10%;
		margin-top: 10px;
		span{
			margin-right: 20px;
			cursor: pointer;
    }
  }
}
</style>
