<template>
 <div>
  <el-badge value="new" class="item">
  <el-button size="small" :plain="true" @click="open">评论</el-button>
</el-badge>
<el-badge value="hot" class="item">
  <el-button size="small" @click="repy">回复</el-button>
</el-badge>
 <el-carousel :interval="4000" type="card" height="400px">
        <el-carousel-item v-for="item in imagesbox" :key="item.id">
           <img :src="item.idView" class="image" size="100% 100%">
        </el-carousel-item>
      </el-carousel>
<br>
<el-tag>了解数据的流程如下</el-tag>

<el-steps :active="active" finish-status="success">
  <el-step title="用户下单完毕"></el-step>
  <el-step title="后台接收数据"></el-step>
  <el-step title="后台进行操作"></el-step>
</el-steps>
<el-button style="margin-top: 12px;" @click="next">下一步</el-button>
</div>

</template>
<style>
.item {
  margin-top: 10px;
  margin-right: 40px;
}
.image{
  width: 610px;
  size:100% 100%;
}
</style>
<script>
  export default {
    data(){
      return {
        imagesbox:[{id:0,idView:"https://img.adidas.com.cn/resources/2020kvbanner/OCT/ninja/feature-pc(26).jpg"},   
           {id:1,idView:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1603532795839&di=845edecfe9be552d701f9427ef2ea8cb&imgtype=0&src=http%3A%2F%2Fcdn6.hbimg.cn%2Fstore%2Ftuku%2F310_999%2Fpiccommon%2F1213%2F12135%2FD525BBC0E487E5D4C5B0A9860E.jpg"},

          {id:2,idView:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1603532938180&di=c204bb923ee85d496af9a27ac7315ed9&imgtype=0&src=http%3A%2F%2Fshihuo.hupucdn.com%2Fucditor%2F20170629%2F750x545_1c88b1a579a6e29f1e67f992b160b498.png%3FimageView2%2F2%2Fw%2F700%2Finterlace%2F1%257cimageMogr2%2Fformat%2Fjpg"}],
           active: 0
      }
    },
    methods: {
      open() {
        this.$message('评论消息在评论列表中');
      },
      repy() {
        this.$alert('暂无回复消息', '回复', {
          confirmButtonText: '确定',
          callback: action => {
            this.$message({
              type: 'info',
              message: `action: ${ action }`
            });
          }
        });
      },
       next() {
        if (this.active++ > 2) this.active = 0;
      }
    }
  }
</script>