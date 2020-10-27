<template>
	<div>
  	<el-table :data="comments" border style="width: 100%">
    <el-table-column prop="id" label="ID" width="180"></el-table-column>
    <el-table-column prop="content" label="评论内容"></el-table-column>
    <el-table-column prop="orderId" label="订单编号"></el-table-column>
    <el-table-column prop="commentTime" label="评论时间"></el-table-column>
    <el-table-column prop="status" label="评论状态"></el-table-column>

	<el-table-column
      fixed="right"
      label="操作"
      width="200">
      <template slot-scope="scope">
        <el-button @click="reHandler(scope.row.id)" type="danger" size="mini">审核拒绝</el-button>
        <el-button @click="doHandler(scope.row.id)" type="success" size="mini">审核通过</el-button>
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
			this.querycomment(this.list);
			console.log(list)
		},
		computed:{
				...mapState("comment",["comments","total"]),
			},
		methods:{
			...mapActions("comment",["findAll","querycomment","savecomment","delcommentById","commentExamine","commentRefuseExamine"]),
			changePage(page){
				this.list.page = page - 1;
				this.querycomment(this.list);
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
				this.savecomment(this.form)
				//关闭模态框
				this.dialogFormVisible = false;
			},
			reHandler(id){
				this.$confirm("此操作将不通过该评论, 是否继续?", "提示", {
					confirmButtonText: "确定",
					cancelButtonText: "取消",
					type: "warning",
				})
					.then(() => {
					this.commentRefuseExamine(id);
					this.$message({
						type: "success",
						message: "拒绝成功!",
					});
					})
					.catch(() => {
					this.$message({
						type: "info",
						message: "已取消拒绝",
					});
					});

			},
			doHandler(id){
				this.$confirm("此操作将通过该评论, 是否继续?", "提示", {
					confirmButtonText: "确定",
					cancelButtonText: "取消",
					type: "warning",
				})
					.then(() => {
					this.commentExamine(id);
					this.$message({
						type: "success",
						message: "通过成功!",
					});
					})
					.catch(() => {
					this.$message({
						type: "info",
						message: "已取消通过",
					});
					});

			},
			searchHandle(){
				this.querycomment(this.list)
			},
			detailsHandle(id){
				this.$router.push({name:"details",params:{id:id}})
			}
		}
	}
</script>
<style scoped>

</style>
