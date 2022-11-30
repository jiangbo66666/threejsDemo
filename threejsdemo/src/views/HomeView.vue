<template>
	<div>
		<div class="switchView" @click="toSun">太阳系</div>
		<div @click="toSave">保存页面</div>
		<!-- 二级路由实现多页面的方便切换 -->
		<router-view/>
	</div>
</template>

<script>
// @ is an alias to /src
import {a} from  '../assets/common'
a()
export default {
	name: 'HomeView',
	components: {
	},
	methods :{
		toSun() {
			this.$router.push('/sunEarth')
		},
		toSave() {
			this.$router.push('/SaveFile')
		}
	},
	created() {
		
	}
}
</script>

<style scoped>
.switchView{
	position: fixed;
	margin: 0;
	padding: 0;
	height: 100px;
}
</style>
