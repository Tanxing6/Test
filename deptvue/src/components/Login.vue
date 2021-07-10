<template>
	<el-form ref="form" :model="form" label-width="80px">
	  <el-form-item label="用户名称">
	    <el-input v-model="Loginform.userName"></el-input>
	  </el-form-item>
	  <el-form-item label="密码">
	    <el-input v-model="Loginform.passWord"></el-input>
	  </el-form-item>
	 <el-button @click="toLogin">登录</el-button>
	  <el-button @click="testJwt">jwt测试</el-button>
	  <el-button @click="testJwtAdd">jwt新增测试</el-button>
	  <el-button @click="testJwtUpdate">修改测试</el-button>
	  <el-button @click="testJwtDel">jwt删除测试</el-button>
	  </el-form>
</template>

<script>
	export default{
		name:"Login",
		data(){
			return{
				Loginform:{
					userName:'',
					passWord:'',
					userId:'1'
				}
			}
		},
		methods:{
			toLogin(){
				this.axios.post("http://localhost:8089/dept/login",this.Loginform)
				.then(function(response){
					sessionStorage.setItem("token",response.data)
					console.log(response)
					
				}).catch(function(error){
					console.log("error",error)
				})
			},
			testJwt(){
				var token=sessionStorage.getItem("token")
				
				this.axios.get("http://localhost:8089/dept/testJwt",{headers:{"JWTDemo":token}})
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log("error",error)
				})
			},
			testJwtAdd(){
				var token=sessionStorage.getItem("token")
				
				this.axios.post("http://localhost:8089/dept/testJwt",{},{headers:{"JWTDemo":token}})
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log("error",error)
				})
			}
			,
			testJwtUpdate(){
				var token=sessionStorage.getItem("token")
				
				this.axios.put("http://localhost:8089/dept/testJwt",{},{headers:{"JWTDemo":token}})
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log("error",error)
				})
			},
			testJwtDel(){
				var token=sessionStorage.getItem("token")
				
				this.axios.delete("http://localhost:8089/dept/testJwt",{headers:{"JWTDemo":token}})
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log("error",error)
				})
			}
			
		}
	}
</script>

<style>
</style>
