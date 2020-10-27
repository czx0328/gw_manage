<template>
	<div>
		<el-button type="primary" size="small" @click="toAddHandler">添加商品种类</el-button>
		<el-input v-model="list.name" clearable placeholder="请输入商品种类名称" style="width:200px;display:inline-block"></el-input>
	<el-button type="primary" size="small" @click="searchHandle">查询</el-button>
	
  	<el-table :data="categorys" border style="width: 100%">
    <el-table-column prop="id" label="ID" width="180"></el-table-column>
    <el-table-column prop="name" label="商品种类名称"></el-table-column>
    <el-table-column prop="num" label="数量"></el-table-column>
    <!-- <el-table-column  prop="parentId" label="该种类主打品牌"></el-table-column> -->
    <el-table-column  prop="icon" label="icon">
		<template slot-scope="scope">
		<el-avatar :size="50" :src="scope.row.icon" alt="图片找不到" style="width:60px;height:60px">
		</el-avatar></template>
		</el-table-column>
	<el-table-column
      fixed="right"
      label="操作"
      width="200">
      <template slot-scope="scope">
		<el-button @click="editHandler(scope.row)" type="text" size="small">编辑</el-button>
        <el-button @click="delHandler(scope.row.id)" type="text" size="small">删除商品种类</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 表格结束 -->
  <!-- 分页 -->
  <el-pagination 
  background 
  layout="prev, pager, next" 
  :total="total"
  :page-size="this.list.pageSize"
  @current-change = "changePage"></el-pagination>
  <!-- 分页结束 -->
  <!-- 模态框开始 -->
  <el-dialog :title="title" :visible.sync="dialogFormVisible">
  <el-form :model="form">
    <el-form-item label="商品种类名称">
      <el-input v-model="form.name" clearable placeholder="请输入商品种类名称"></el-input>
    </el-form-item>
	<el-form-item label="商品种类icon地址">
      <el-input v-model="form.icon" clearable placeholder="请输入icon地址"></el-input>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="submit()">确 定</el-button>
  </div>
</el-dialog>
  <!-- 模态框结束 -->
	</div>
</template>
<script>
import { mapState, mapActions } from "vuex";
	export default{
		data(){
			return {
				 circleUrl: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1277861296,2710786810&fm=26&gp=0.jpg",
				list:{
					page:0,
					pageSize:5,
				},
				//模态框隐藏
				dialogFormVisible:false,
				form:{
				},
				title:''
				
			}
		},
		created(){
			// this.findAll();
			// 分页查询顾客信息
			this.querycategory(this.list);
			console.log(list)
		},
		computed:{
				...mapState("category",["categorys","total"]),
			},
		methods:{
			...mapActions("category",["findAll","querycategory","savecategory","delcategoryById"]),
			changePage(page){
				this.list.page = page - 1;
				this.querycategory(this.list);
			},
			toAddHandler(){
				//控制模态框打开
				this.dialogFormVisible = true;
				this.title = "添加商品种类"
				//清空表单
				this.form = {}
			},
			submit(form){
				//调用store中的方法提交请求
				this.savecategory(this.form)
				//关闭模态框
				this.dialogFormVisible = false;
			},
			delHandler(id){
				this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
					confirmButtonText: "确定",
					cancelButtonText: "取消",
					type: "warning",
				})
					.then(() => {
					this.delcategoryById(id);
					this.$message({
						type: "success",
						message: "删除成功!",
					});
					})
					.catch(() => {
					this.$message({
						type: "info",
						message: "已取消删除",
					});
					});

			},
			editHandler(row){
				//打开模态框
				this.dialogFormVisible = true;
				this.title = "修改商品种类信息"
				this.form = row
			},
			searchHandle(){
				this.querycategory(this.list)
			},
			detailsHandle(id){
				this.$router.push({name:"details",params:{id:id}})
			}
		}
	}
</script>
<style scoped>

</style>
