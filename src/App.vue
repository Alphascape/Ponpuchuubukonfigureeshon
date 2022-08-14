<template>
	<div id="app">
		<div>
		<el-button @click="ccc">看看</el-button>
		<div class='config_menu'>
			<draggable v-model="data.config" class="config_menu_drag" >
				<div  class="config_menu_item" v-for="(item, index) in data.config" :key='item.nodeName' @click="setPartArr(item,index)">
					{{item.nodeName}}
					<i  class="el-icon-close" v-if="index==data.config.length-1" @click.stop="delNode(item,index)"></i>
				</div>
			</draggable>
			<div class="config_menu_addNode" @click="addNode()">
				<i class="el-icon-plus"></i>
			</div>
			</div>
			</div>
		<el-card style="min-height: 700px;" >
			<div class="part_menu">
				<div >对应节点：{{partObj.nodeName}}</div>
				<draggable v-model="partObj.partArr" class="part_menu_drag" v-if="partObj.nodeName" @end='rebind' >
					<div v-for="(item, index) in partObj.partArr" class="part_menu_item" :key='item.id'>
						第{{index+1}}根：{{item.name}}
						<i  class="el-icon-close" style="font-size: 20px;position: absolute;right: 0px;" @click="delPart(index)"></i>
					</div>
				</draggable>
				<div class="part_menu_item" @click="addPart" >
					<i class="el-icon-plus"></i>
				</div>
			</div>

		</el-card>
	</div>
</template>

<script>
import draggable from 'vuedraggable';
export default {
	name: 'app',
	data() {
		return {
			partObj:{//零件对象
				nodeName:'',//对应节点
				partArr:[]//零件列表
			},
			data: {//数据
				nodeNum: 2,//节点数
				config: [
					//配置列表
					{
						nodeName:'初始1节点',
						partNum: 1, //零件数量
						partArr: [
							//零件列表
							{
								id: 74,
								name: '1节点5米直管'
							}
						]
					},
					{
						nodeName:'初始2节点',
						partNum: 2, //零件数量
						partArr: [
							//零件列表
							{
								id: 74,
								name: '2节点5米直管'
							},
							{
								id: 75,
								name: '2节点10米直管'
							}
						]
					}
				]
			}
		};
	},
	methods:{
		ccc(){
			console.log(JSON.stringify(this.data));
		},
		//重新绑定
		rebind(){
			console.log(this.partObj,this.data);
			// for(let key =0;key<this.data.config.length;key++){
			// 	// debugger
			// 	let value = this.data.config[key];
			// 	if(value.nodeName==this.partObj.nodeName){
			// 		value.partArr=this.partObj.partArr;
			// 	}
			// }
		},
		//添加节点
		addNode(){
			this.data.nodeNum++;
			this.data.config.push({
				nodeName:'新初始节点'+this.data.nodeNum,
				partNum: 0, //零件数量
				partArr: []
			})
		},
		//删除节点
		delNode(item,index){
			this.data.config.splice(index,1);
			this.data.nodeNum--;
			//如果删除下面零件列表对应的节点,对应节点变为0
			if(this.partObj.nodeName==item.nodeName){
				this.partObj.nodeName='';
				this.partObj.partArr=[];
			}
			
		},
		//设置零件列表
		setPartArr(item,index){
			// this.partObj.nodeName=item.nodeName;
			// this.partObj.partArr=item.partArr;
			this.partObj=item;
		},
		//添加零件
		addPart(){
			this.partObj.partArr.push({
				name:this.partObj.nodeName+'节点新加零件'+Math.random()
			});
			this.partObj.partNum++;
		},
		//删除零件
		delPart(index){
			this.partObj.partArr.splice(index,1);
			this.partObj.partNum--;
		}
	},
	mounted(){
		//初始化是data第一个数据，懒得弄axios了
		this.partObj.nodeName=this.data.config[0].nodeName;
		this.partObj.partArr=this.data.config[0].partArr;
	},
	components: {
		draggable
	}
};
</script>

<style>
	
.config_menu {
	background-color: skyblue;
	width: 100%;
	height: 50px;
	display: flex;
	/* justify-content: center; */
	align-items: center;
}
.config_menu_drag{
	background-color: skyblue;
	/* width: 100%; */
	height: 50px;
	display: flex;
	/* justify-content: center; */
	align-items: center;
}
.config_menu_item {
	width: 150px;
	height: 40px;
	background-color: aliceblue;
	margin: 10px;
	display: flex;
	justify-content: center;
	align-items: center;
	cursor: pointer;
}
.config_menu_addNode{
	width: 150px;
	height: 40px;
	background-color: aliceblue;
	margin: 10px;
	display: flex;
	justify-content: center;
	align-items: center;
	cursor: pointer;
	
}
.part_menu{
	/* background-color: ; */
	height: 500px;
	width: 100%;
	display: flex;
	flex-direction: column;
	align-items: center;
	
}
.part_menu_drag{
	/* background-color: ; */
	/* height: 500px; */
	width: 100%;
	display: flex;
	flex-direction: column;
	align-items: center;
	
}
.part_menu_item{
	position: relative;
	width: 300px;
	height: 50px;
	background-color: aliceblue;
	margin: 10px;
	display: flex;
	justify-content: center;
	align-items: center;
	/* font-size: px; */
	cursor: pointer;
}

</style>
