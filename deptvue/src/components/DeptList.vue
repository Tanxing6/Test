<template>
	<!-- prop对应json的属性 -->
	<el-button @click="dialogFormVisible=true">新增</el-button>
	<el-dialog title="添加部门" v-model="dialogFormVisible">
		<el-form :model="form">
			<el-form-item label="部门编号" :label-width="formLabelWidth">
				<el-input v-model="form.deptno " autocomplete="off"></el-input>
			</el-form-item>
			<el-form-item label="部门名称" :label-width="formLabelWidth">
				<el-input v-model="form.dname" autocomplete="off"></el-input>
			</el-form-item>
			<el-form-item label="部门地址" :label-width="formLabelWidth">
				<el-input v-model="form.loc" autocomplete="off"></el-input>
			</el-form-item>
		</el-form>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="dialogFormVisible = false">取 消</el-button>
				<el-button type="primary" @click="addDept ">确 定</el-button>
			</span>
		</template>
	</el-dialog>
<el-dialog title="修改部门信息" v-model="dialogFormVisible2">
		<el-form :model="form">
			<el-form-item label="部门编号" :label-width="formLabelWidth">
				<el-input v-model="form.deptno" disabled></el-input>
			</el-form-item>
			<el-form-item label="部门名称" :label-width="formLabelWidth">
				<el-input v-model="form.dname" autocomplete="off"></el-input>
			</el-form-item>
			<el-form-item label="部门地址" :label-width="formLabelWidth">
				<el-input v-model="form.loc" autocomplete="off"></el-input>
			</el-form-item>
		</el-form>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="dialogFormVisible2 = false">取 消</el-button>
				<el-button type="primary" @click="updateDept ">确 定</el-button>
			</span>
		</template>
	</el-dialog>

	<el-table :data="DeptData" border style="width: 100%">
		<el-table-column fixed prop="deptno" label="部门编号" width="150">
		</el-table-column>
		<el-table-column prop="dname" label="部门名称" width="120">
		</el-table-column>
		<el-table-column prop="loc" label="地址" width="120">
		</el-table-column>
		<el-table-column label="操作" width="100">
			<!-- #:v-slot 插槽
				scope定义对象的名字-->
			<template #default="scope">
				<el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
				<el-button type="text" size="small" @click="showEidt(scope.row)">编辑</el-button>
				<el-button type="text" size="small" @click="delDept(scope.row)">删除</el-button>
			</template>
		</el-table-column>
	</el-table>

	<!--  -->
</template>

<script>
	export default {
		name: "DeptList",
		data() {
			return {
				DeptData: [],
				dialogFormVisible: false,
				dialogFormVisible2: false,
				// 文本框大小
				formLabelWidth: "100px",
				form: {
					deptno: "",
					dname: "",
					loc: ""
				}
			}
		},
		methods: {
			handleClick(row) {
				console.log(row);
			},
				// _this.dialogFormVisible = false
			addDept() {
				var token=sessionStorage.getItem("token")
					const _this = this
					this.axios.post("http://localhost:8089/dept/dept", this.form,{headers:{"JWTDemo":token}})
					.then(function(response) {
						console.log(response)
						var dept=response.data
						_this.DeptData.push(dept)
					_this.dialogFormVisible = false
						
					}).catch(function(error) {
						console.log(error)
					})
			},
			showEidt(row){
				// 传当前行的值
				this.form.deptno=row.deptno
				this.form.dname=row.dname
				this.form.loc=row.loc
				this.dialogFormVisible2 = true
				
			},
			updateDept() {
				var token=sessionStorage.getItem("token")
					const _this = this
					this.axios.put("http://localhost:8089/dept/dept", this.form,{headers:{"JWTDemo":token}})
					.then(function(response) {
						console.log(response)
						var dept=response.data
						// _this.DeptData.push(dept)
					var row=_this.DeptData.filter(d=>d.deptno==dept.deptno)[0]
					row.dname=dept.dname
					row.loc=dept.loc
					_this.dialogFormVisible2 = false
					// for (var i = 0; i < _this.DeptData.length; i++) {
					// 	if (_this.DeptData[i].id == dept.deptno) {
					// 		_this.DeptData[i] = dept;
					// 		_this.dialogFormVisible2 = false
					// 	}
					// }
						
					}).catch(function(error) {
						console.log(error)
					})
			},
			delDept(row) {
				var token=sessionStorage.getItem("token")
							const _this = this
							var flag=true
							this.$confirm('此操作将永久删除该数据, 是否继续?', '提示', {
							          confirmButtonText: '确定',
							          cancelButtonText: '取消',
							          type: 'warning'
							        }).then(() => {
										_this.axios.delete("http://localhost:8089/dept/dept/"+row.deptno,{headers:{"JWTDemo":token}})
											.then(function(response) {
												var dept = response.data
												var rows = _this.DeptData
													.filter(d => d.deptno != row.deptno)
												console.log("del rows:%o",rows)
												_this.DeptData=rows
											}).catch(function(error) {
												console.log(error)
											})
							        }).catch(() => {
										this.$message({
										  type: 'error',
										  message: '取消删除!'
										});
							        });	
						}

	},
	created() {
		var token=sessionStorage.getItem("token")
		const _this = this;
		this.axios.get("http://localhost:8089/dept/findDepts",{headers:{"JWTDemo":token}})
			.then(function(response) {
				_this.DeptData = response.data
				console.log(response)
			}).catch(function(error) {
				console.log(error)
			})
			
	}
	}
</script>

<style>
</style>
