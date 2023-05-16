<template>
	<view class="home">
		<!-- 导航烂 -->
		<scroll-view scroll-x="true" class="nav">
			<view class="item" 
			:class="navActive === index?'active':''" 
			v-for="(item,index) in navListData"
			:key="item.id"
			@click="navSelect(index,item.id)"
			>
			{{item.classname}}
		</view>
		</scroll-view>
		
		<!-- 新闻列表 -->
		<view class="contentBox">
			<view class="newsItem" v-for="item in newsListData">
				<newsBox :new-info="item"></newsBox>
			</view>
		</view>
		
		<!-- 无数据时 -->
		<view class="noData" v-if="!newsListData.length">
			<image src="../../static/images/nodata.png" mode="aspectFit"></image>
		</view>
		
		<!-- loading -->
		<view class="loading" v-if="newsListData.length">
			<view v-if="loading === 1">加载中.....</view>
			<view v-if="loading === 2 & currentPage !== 1">没有啦~~</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				navActive:0,
				navListData:[],
				newsListData:[],
				currentPage:1,
				loading:0,	// 0默认 1加载更多 2没有数据
			}
		},
		onLoad() {
			this.getNavListData();
			this.getNewsListData();
		},
		onReachBottom(){	// 上拉刷新
			if(this.loading === 2){
				return
			};
			this.loading = 1;
			this.currentPage++;
			this.getNewsListData();
		},
		methods: {
			// 导航栏点击事件
			navSelect(index,cid){
				this.currentPage = 1;	// 初始化页码
				this.navActive = index;
				this.newsListData = []; // 清空数据
				this.getNewsListData(cid);
			},
			
			// 获取导航列表数据
			getNavListData(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success:res=>{
						this.navListData = res.data;
					}
				})
			},
			
			// 获取新闻列表数据
			getNewsListData(cid = 50){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					data:{
						cid,
						page:this.currentPage
					},
					success:res=>{
						this.newsListData = [...this.newsListData,...res.data];
							
						if(!res.data.length){
							this.loading = 2;
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
.home{
	.nav{
		height: 100rpx;
		background-color: #f7f8fa;
		white-space: nowrap;
		position: fixed;
		z-index: 100;
		top:var(--window-top);
		left: 0;
		/deep/ ::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}
		
		.item{
			display: inline-block;
			font-size: 32rpx;
			padding: 0 30rpx;
			color: #333;
			line-height: 100rpx;
			&.active{
				color: #31C27C;
			}
		}
	}
	.contentBox{
		margin-top: 100rpx;
		padding: 12rpx;
		.newsItem{
			border-bottom: 1rpx #999 dotted;
			
		}
	}
	.noData{
		display: flex;
		justify-content: center;
		image{
			width: 360rpx;
		}
	}
	.loading{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		color: #838383;
		font-size: 24rpx
	}
}
</style>
