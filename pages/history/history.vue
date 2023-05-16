<template>
	<view class="history">
		<view class="top">
			<image src="../../static/images/history.png" mode="aspectFill"></image>
			<text>浏览历史</text>
		</view>
		<view class="contentBox">
			<view class="newsItem" v-for="item in historyDataList">
				<newsBox :new-info="item"></newsBox>
			</view>
		</view>
		<view class="noHistory" v-if="!historyDataList.length">
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<view class="txt">暂无浏览历史记录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyDataList:[]
			};
		},
		onShow(){
			this.getHistoryStorageData();
		},
		methods:{
			// 获取浏览历史数据缓存记录
			getHistoryStorageData(){
				this.historyDataList=uni.getStorageSync("historyData");
			}
		}
	}
</script>

<style lang="scss">
.history{
	.top{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 24rpx 0;
		background-color: #f8f8f8;
		height: 300rpx;
		image{
			height: 100rpx;
			width: 100rpx;
		}
		text{
			margin: 24rpx 0;
		}
	}
	.contentBox{
		padding: 12rpx;
		.newsItem{
			border-bottom: 1rpx #999 dotted;
		}
	}
	.noHistory{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 400rpx;
		}
		txt{
			font-size: 16rpx
		}
	}
}
</style>
