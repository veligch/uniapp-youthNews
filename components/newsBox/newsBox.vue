<template>
	<view class="newsBox" @click="skipDetail">
		<view class="img">
			<image :src="newInfo.picurl" mode="aspectFill"></image>
		</view>
		<view class="info">
			<text class="title">{{newInfo.title}}</text>
			
			<view>
				<text class="txt" v-if="!newInfo.lookTime">{{newInfo.author}} {{newInfo.hits}}浏览</text>
				<text class="txt" v-if="newInfo.lookTime">浏览时间：{{newInfo.lookTime}}</text>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		name:"newsBox",
		props:{
			newInfo:{
				type:Object,
				default(){
					return {
						id:"",
						cid:"",
						title:"新闻标题新闻标题新闻标题新闻标题新闻标题新闻标题新闻标题新闻标题新闻标题",
						picurl:"../../static/images/nopic.jpg",
						lookTime:"",
						hits:100,
						author:"张三"
					}
				}
			}
		},
		data() {
			return {
				
			};
		},
		methods:{
			skipDetail(){
				console.warn(this.newInfo);
				let id = this.newInfo.id;
				let cid = this.newInfo.classid;
				uni.navigateTo({
					url:`/pages/detail/detail?id=${id}&cid=${cid}`
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
.newsBox{
	padding: 12rpx;
	display: flex;
	
	.info{
		flex: 1;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		padding: 12rpx;
		
		.title{
			font-size: 32rpx;
			//两行或多行显示
			text-overflow: -o-ellipsis-lastline;
			overflow: hidden;				//溢出内容隐藏
			text-overflow: ellipsis;		//文本溢出部分用省略号表示
			display: -webkit-box;			//特别显示模式
			-webkit-line-clamp: 2;			//行数
			line-clamp: 2;					
			-webkit-box-orient: vertical;	//盒子中内容竖直排列
		}
		.txt{
			font-size: 16rpx;
			color: #666;
		}
	}
	.img>image{
		width: 230rpx;
		height: 160rpx;
	}
}
</style>