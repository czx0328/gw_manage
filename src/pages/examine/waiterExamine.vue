<template>
	<div>
<el-progress type="circle" :percentage="0"></el-progress>
<el-progress type="circle" :percentage="25"></el-progress>
<el-progress type="circle" :percentage="100" status="success"></el-progress>
<el-progress type="circle" :percentage="70" status="warning"></el-progress>
<el-progress type="circle" :percentage="50" status="exception"></el-progress>
<br>
<p>鉴别员差评数据&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/鉴别员未完成度&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
	/资料完整度&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/好评比例&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/差评比例</p>
	</div>
</template>
<style scoped>
div{
	text-align: center;
}
</style>