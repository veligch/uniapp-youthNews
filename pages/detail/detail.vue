<template>
	<view class="detail">
		<view class="title">{{newsInfoData.title}}</view>
		<view class="info">
			<text>编辑：{{newsInfoData.author}}</text>
			<text>发布时间：{{newsInfoData.posttime}}</text>
		</view>
		<view class="content">
			<rich-text :nodes="newsInfoData.content"></rich-text>
		</view>
	</view>
</template>

<script>
import { parseTime } from '@/utils/time.js'
	export default {
		props:{
			cid:{
				type:Number,
				default:0
			}
		},
		data() {
			return {
				option:null,
				newsInfoData:{}
			};
		},
		onLoad(e){
			console.log(e);
			this.options = e;
			this.getNewsInfoData();
		},
		methods:{
			// 获取新闻详情内容
			getNewsInfoData(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
					data:{...this.options},
					success:(res)=>{
						console.log(res.data);
						res.data.content = res.data.content.replace(/<img/gi,`<img style="max-width:100%"`);
						this.newsInfoData = res.data;
						this.newsInfoData.posttime = parseTime(this.newsInfoData.posttime);
						
						this.saveHistoryData();
					}
				})
			},
			
			saveHistoryData(){
				let {id,classid,picurl,title} = this.newsInfoData;
				let item = {
					id,
					classid,
					picurl,
					title,
					lookTime:parseTime(Date.now())
				};
				let historyStorage = uni.getStorageSync("historyData") || [];
				historyStorage = historyStorage.filter((item=>item.id !== this.newsInfoData.id));
				historyStorage.unshift(item);
				historyStorage.slice(0,10);
				uni.setStorageSync('historyData', historyStorage);
			}
		}
	}
</script>

<style lang="scss" scoped>
.detail{
	padding: 24rpx 12rpx;
	.title{
		font-size: 36rpx;
		
	}
	.info{
		font-size:  16rpx;
		color: #999;
		background-color: #F6F6F6;
		border-radius: 16rpx;
		padding: 12rpx;
		margin: 30rpx 0;
		display: flex;
		justify-content: space-between;
	}
	.content{
		/deep/ img{
			max-width: 100%;
		}
	}
}
</style>
