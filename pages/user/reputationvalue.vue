<template>
	<view class="uni-page-body">

		<view class="header-box">
			
			<image src="/static/img/goods/p1.jpg" class="img"></image>
			<view class="uni-flex pinfen">
				<uni-rate class="rate" :size="12" :value="5" />
				<text class="list-succ">3.15分</text>
			</view>
			
			<image class="xingyu" src="/static/img/goods/p10.jpg"></image>
			
		</view>
		<uni-list>
			<uni-list-item title="个人资料" :showArrow="false" :showBadge="true">
				<text>1.5分 </text>
			</uni-list-item>
			<uni-list-item title="从业年限" :showArrow="false" :showBadge="true">
				<text>0.15分 </text>
			</uni-list-item>
			<uni-list-item title="保证金" :showArrow="false" :showBadge="true">
				<text>0分 </text>
			</uni-list-item>
			<uni-list-item title="服务综合评价" :showArrow="false" :showBadge="true">
				<text>1.5分 </text>
			</uni-list-item>
		</uni-list>

		<view class="uni-page-footer" @tap="nto">
			评定方法 | 联系客服
		</view>

	</view>
</template>
<script>
	import uniRate from '@/components/uni-rate/uni-rate.vue'
	import uniIcons from "@/components/uni-icons/uni-icons.vue"
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'
	export default {
		components: {
			uniList,
			uniListItem,
			uniIcons,
			uniRate
		},
		data() {
			return {
			}
		},
		computed: {
		
		},
		methods: {
			nto() {
				uni.navigateTo({
					url: '/pages/user/reputationrun'
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.uni-page-body {
		.header-box {
			background:linear-gradient(175deg,rgba(61,140,243,1) 0%,rgba(116,227,255,1) 100%);
			padding: 40upx;
			margin-bottom: 24upx;
			position: relative;
			.xingyu {
				position: absolute;
				right: 18upx;
				bottom: 58upx;
				height: 148upx;
				width: 275upx;
			}
			.pinfen {
				align-items: center;
				background-color: rgba($color: #ffffff, $alpha: 0.7);
				padding: 0px 10upx;
				border-radius: 10upx;
				width: 240upx;
				display: flex;
				justify-content: center;
			}
			.img {
				height: 142upx;
				width: 142upx;
				margin-bottom: 8upx;
				border-radius: 50%;
			}

			.price-box {
				color: #FFFFFF;
				font-size: 56upx;

			}

			.uni-icon-clear,
			.uni-icon-eye {
				color: #999;
			}
		}

		.uni-page-footer {
			position: fixed;
			bottom: 50upx;
			left: 0upx;
			right: 0upx;
			text-align: center;
			color: #1F96F7;
			font-size: 22upx;

			.icon {
				margin-right: 10upx
			}
		}
	}
</style>
