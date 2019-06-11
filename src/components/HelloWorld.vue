<template>
	<div class="hello">

	 <el-carousel :interval="4000" type="card" height="200px">
			<el-carousel-item v-for="item in bannerData" :key="item.id">
				<h3 class="medium"><img :src="item.imgUrl" alt=""></h3>
			</el-carousel-item>
		</el-carousel>
		
		<el-menu :default-active="activeIndex2" class="el-menu-demo" mode="horizontal" @select="handleSelect"
		 background-color="#545c64" text-color="#fff" active-text-color="#ffd04b">
			<el-menu-item index="1">新闻管理</el-menu-item>
			<el-submenu index="2">
				<template slot="title">我的工作台</template>
				<el-menu-item index="2-1">选项1</el-menu-item>
				<el-menu-item index="2-2">选项2</el-menu-item>
				<el-menu-item index="2-3">选项3</el-menu-item>
				<el-submenu index="2-4">
					<template slot="title">选项4</template>
					<el-menu-item index="2-4-1">选项1</el-menu-item>
					<el-menu-item index="2-4-2">选项2</el-menu-item>
					<el-menu-item index="2-4-3">选项3</el-menu-item>
				</el-submenu>
			</el-submenu>
			<el-menu-item index="3" disabled>消息中心</el-menu-item>
			<el-menu-item index="4"><a href="https://www.ele.me" target="_blank">订单管理</a></el-menu-item>
		</el-menu>





		<el-table v-loading="loading" :data="tableData" style="width: 100%">
			<el-table-column prop="time" label="日期" width="180">
			</el-table-column>
			<el-table-column prop="title" label="新闻标题" width="500">
			</el-table-column>
			<el-table-column prop="id" label="标号">
			</el-table-column>
			<el-table-column prop="id" label="展示板块">
			</el-table-column>
			<el-table-column prop="id" label="所属分类">
			</el-table-column>
			<el-table-column label="操作">

				<el-button-group>
					<el-button type="primary" icon="el-icon-edit"></el-button>
					<el-button type="primary" icon="el-icon-share"></el-button>
					<el-button type="primary" icon="el-icon-delete" @click="del"></el-button>
				</el-button-group>

			</el-table-column>
		</el-table>
	</div>
</template>

<script>
	export default {
		name: 'HelloWorld',
		data() {
			return {
				 activeIndex: '1',
        activeIndex2: '1',
				loading: true,
				tableData: [],
				bannerData:[]
			}
		},
		methods: {
			//获取新闻列表数据
			getNewList() {
				let that = this;
				this.$http.get('https://www.easy-mock.com/mock/5ce95f110c7bee34a01eb2d0/vuetest/newlist').then(res => {
					this.tableData = res.data.data.newlist;
					setTimeout(function() {
						that.loading = false;
					}, 3000)

				});
			},
			del() {
				this.$confirm('此操作将永久删除词条数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http.post('url', {
						id: 1
					}).then(res => {
						if (res.code == 20) {
							this.$message({
								type: 'success',
								message: '删除成功!',
								showClose: true
							});
						}
					});

				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除',
						showClose: true
					});
				});
			},
			handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
			//bannerdata
			getBannerList(){
				this.$http.get('https://www.easy-mock.com/mock/5c73b09c0163442f050808c0/home/banner').then(res=>{
					console.log(res);
					this.bannerData=res.data.data
				});
			}
		},
		mounted() {
			this.getBannerList();
			this.getNewList();
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }
  .el-carousel__item h3 img{
		display: block;
		width: 100%;
	}
  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
</style>
