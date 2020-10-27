<template>
	<div>
		<el-button type="primary" size="small" @click="toAddHandler">添加商品</el-button>
		<el-input v-model="list.name" clearable placeholder="请输入商品名称" style="width:200px;display:inline-block"></el-input>
		<el-button type="primary" size="small" @click="searchHandle">查询</el-button>
  	<el-table :data="products" border style="width: 100%">
		<el-table-column prop="id" label="ID" width="180"></el-table-column>
		<el-table-column prop="name" label="商品名称"></el-table-column>
		<el-table-column prop="description" label="描述"></el-table-column>
		<el-table-column  width="100px" prop="status" label="品牌"></el-table-column>
		<el-table-column  width="50px" prop="price" label="价格"></el-table-column>
		<el-table-column  width="50px" prop="categoryId" label="原价"></el-table-column>
		<el-table-column  prop="photo" label="照片" >
			<template slot-scope="scope">
				<img :src="scope.row.photo" alt="图片找不到" style="width:130px;height:70px">
			</template>
		</el-table-column>
		<el-table-column
		fixed="right"
		label="操作"
		width="200">
		<template slot-scope="scope">
			<el-button @click="editHandler(scope.row)" type="text" size="small">编辑</el-button>
			<el-button @click="delHandler(scope.row.id)" type="text" size="small">下架</el-button>
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
    <el-form-item label="商品名称">
      <el-input v-model="form.name" clearable placeholder="请输入商品名称"></el-input>
    </el-form-item>
	<el-form-item label="商品描述">
      <el-input v-model="form.description" clearable placeholder="请输入商品描述"></el-input>
    </el-form-item>
	<el-form-item label="商品价格">
      <el-input  v-model="form.price" clearable placeholder="请输入商品价格"></el-input>
    </el-form-item>
	<el-form-item label="商品品牌">
    <el-input  v-model="form.status" clearable placeholder="请输入商品品牌"></el-input>
	</el-form-item>
   <el-form-item label="商品照片地址">
      <el-input v-model="form.photo" clearable placeholder="请输入商品照片地址"></el-input>
    </el-form-item>
	<el-form-item label="原价">
      <el-input  v-model="form.categoryId" clearable placeholder="请输入商品原价"></el-input>
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
				list:{
					page:0,
					pageSize:5,
				},
				//模态框隐藏
				dialogFormVisible:false,
				form:{
				},
				tltle:""
			}
		},
		created(){
			// this.findAll();
			// 分页查询顾客信息
			this.queryproduct(this.list);
			console.log('11111'+this.products)
		},
		computed:{
				...mapState("product",["products","total"]),
			},
		methods:{
			...mapActions("product",["findAll","queryproduct","saveproduct","delproductById"]),
			changePage(page){
				this.list.page = page - 1;
				this.queryproduct(this.list);
			},
			toAddHandler(){
				//控制模态框打开
				this.dialogFormVisible = true;
				this.title = "添加商品"
				//清空表单
				this.form = {}
			},
			submit(form){
				//调用store中的方法提交请求
				this.saveproduct(this.form)
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
					this.delproductById(id);
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
				this.title = "修改商品信息"
				this.form = row
			},
			searchHandle(){
				this.queryproduct(this.list)
			},
			detailsHandle(id){
				this.$router.push({name:"details",params:{id:id}})
			}
		}
	}
</script>
<style scoped>

</style>
