<template>
	<view class="uni-page-body header-page-body">
		<view class="header-box">
			<view class="item" :class="isactive?'active':''"  @tap="changepage(true)" >
				缴纳
			</view>
			<view class="item" :class="!isactive?'active':''"  @tap="changepage(false)" >
				转出
			</view>
		</view>
		<view class="container">
			<input-bond ref="sellpro" v-if="isactive"></input-bond>
			<out-bond ref="buypro" v-if="!isactive"></out-bond>
		</view>
	</view>
</template>
<script>
	import inputBond from "./bondchild/inputbond.vue"
	import outBond from "./bondchild/outbond.vue"
	export default {
		components: {
			inputBond,
			outBond
		},
		data() {
			return {
				isactive: true,
			}
		},
		methods: {
			changepage(flag) {
				this.isactive = flag;
			},
			getmore() {
				
			}
		}
	}
</script>
<style lang="scss" scoped>
	.header-box {
		width: 100%;
		padding: 0 4%;
		height: 100upx;
		display: flex;
		align-items: center;
		justify-content: space-around;

		.item {
			color: #212121;
			font-size: 38upx;
			padding: 0 20upx;

			&.active {
				border-bottom: 4upx solid $font-color-light;
			}
		}
		
	}
</style>
