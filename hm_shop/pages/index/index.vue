<template>
	<view class="home">
		<swiper indicator-dots circular>
			<swiper-item v-for="item in swipers" :key="item.id">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
		<!-- 导航区域 -->
		<view class="nav">
			<view class="nav_item" v-for="(item,index) in navs" :key="index" @click="navItemClick(item.path)">
				<view :class="item.icon"></view>
				<text>{{item.title}}</text>
			</view>
		</view>
		<!-- 推荐商品 -->
		<view class="hot_goods">
			<view class="tit">推荐商品</view>
			<goods-list @goodsItemClick="goGoodsDetail" :goods="goods"></goods-list>
		</view>
	</view>
</template>

<script>
	import goodsList from '../../components/goods-list/goods-list.vue'
	export default {
		data() {
			return {
				swipers:[],
				goods:[],
				navs:[
				{
					icon:'iconfont icon-ziyuan',
					title:'黑马超市',
					path:'/pages/goods/goods'
				},
				{
					icon:'iconfont icon-guanyuwomen',
					title:'联系我们',
					path:'/pages/contact/contact'
				},
				{
					icon:'iconfont icon-tupian',
					title:'社区图片',
					path:'/pages/pics/pics'
				},
				{
					icon:'iconfont icon-shipin',
					title:'学习视频',
					path:'/pages/videos/videos'
				},
				]
			}
		},
		onLoad(){
			this.getSwpier()
			this.getHotGoods()
		},
		components:{"goods-list":goodsList},	//注册组件
		methods: {
			//获取轮播图的数据
			async getSwpier(){
				// console.log('获取轮播图数据')
				// uni.request({
				// 	url:'http://localhost:8082/api/getlunbo',
				// 	success:res=>{
				// 		console.log(res)
				// 		if(res.data.status !== 0){
				// 			return uni.showToast({
				// 				title:'获取数据失败'
				// 			})
				// 		}
				// 		this.swipers = res.data.message
				// 	}
				// })
				const res = await this.$myRuquest({
					url: '/api/getlunbo'
				})
				this.swipers = res.data.message
				// console.log(res)
			},
			// 获取热门商品列表数据
			async getHotGoods(){
				const res = await this.$myRuquest({
					url:'/api/getgoods?pageindex=1'
				})
				// console.log(res)
				this.goods = res.data.message
			},
			// 导航点击的处理函数
			navItemClick(url){
				// console.log('跳转了',url)
				uni.navigateTo({
					url
				})
			},
			// 导航到商品详情页
			goGoodsDetail(id){
				uni.navigateTo({
					url:'/pages/goods-detail/goods-detail?id='+id
				})
			}
		}
	}
</script>

<style lang="scss">
	.home{
		swiper{
			width: 750rpx;	//占据一整屏
			height: 380rpx;
			image{	//swiper-item会自动设置100%
				width: 100%;
				height: 100%;
			}
		}
		.nav{
			display: flex;
			.nav_item{
				width: 25%;
				text-align: center;
				view{
					width: 120rpx;
					height: 120rpx;
					background: $shop-color;
					border-radius: 60%;
					margin: 10px auto;
					line-height: 120rpx;
					color: #FFFFFF;
					font-size: 50rpx;
				}
				.icon-tupian{
					font-size: 45rpx;
				}
				text{
					font-size: 30rpx;
				}
			}
		}
		.hot_goods{
			background: #eee;
			overflow: hidden;	//高度塌陷
			margin-top: 10px;
			.tit{
				height: 50px;
				line-height: 50px; //设置字的行高
				color: $shop-color;
				text-align: center;
				letter-spacing: 20px; //字的间距
				background: #FFF;
				margin: 7rpx 0;
			}
			
			
		}
	}
</style>
