<template>
	<view class="uni-rate">
		<view :key="index" :style="{ marginLeft: margin + 'px' }" @click="_onClick(index)" class="uni-rate__icon" v-for="(star, index) in stars">
			<!-- <uni-icons :color="color" :size="size" :type="isFill ? 'star-filled' : 'star'" /> -->
			<view class="iconfont icon-xingxingxuanzhong" :style="{color:value >= index ? activeColor : color}"></view>
		</view>
	</view>
</template>

<script>
	import uniIcons from "../uni-icons/uni-icons.vue";
	export default {
		name: "UniRate",
		components: {
			uniIcons
		},
		props: {
			isFill: {
				// 星星的类型，是否镂空
				type: [Boolean, String],
				default: false
			},
			color: {
				// 星星的颜色
				type: String,
				default: "#ececec"
			},
			activeColor: {
				// 星星选中状态颜色
				type: String,
				default: "#18C02C"
			},
			size: {
				// 星星的大小
				type: [Number, String],
				default: 14
			},
			value: {
				// 当前评分
				type: [Number, String],
				default: 0
			},
			max: {
				// 最大评分
				type: [Number, String],
				default: 5
			},
			margin: {
				// 星星的间距
				type: [Number, String],
				default: 0
			},
			disabled: {
				// 是否可点击
				type: [Boolean, String],
				default: false
			}
		},
		data() {
			return {
				valueSync: ""
			};
		},
		computed: {
			stars() {
				const value = this.valueSync ? this.valueSync : 0;
				const starList = [];
				const floorValue = Math.floor(value);
				const ceilValue = Math.ceil(value);
				// console.log("ceilValue: " + ceilValue);
				// console.log("floorValue: " + floorValue);
				for (let i = 0; i < this.max; i++) {
					if (floorValue > i) {
						starList.push({
							activeWitch: "100%"
						});
					} else if (ceilValue - 1 === i) {
						starList.push({
							activeWitch: (value - floorValue) * 100 + "%"
						});
					} else {
						starList.push({
							activeWitch: "0"
						});
					}
				}
				return starList;
			}
		},
		created() {
			this.valueSync = Number(this.value);
		},
		methods: {
			_onClick(index) {
				if (this.disabled) {
					return;
				}
				this.valueSync = index + 1;
				this.$emit("change", {
					value: this.valueSync
				});
			}
		}
	};
</script>

<style scoped lang="scss" >
	.uni-rate {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		line-height: 0;
		font-size: 0;
		flex-direction: row;
		height: 40upx;
	}

	.uni-rate__icon {
		position: relative;
		line-height: 0;
		font-size: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		.iconfont {
			line-height: 0;
			font-size: 10px;
			transform: scale(0.8);
		}
	}

	.uni-rate__icon-on {
		overflow: hidden;
		position: absolute;
		top: 0;
		left: 0;
		line-height: 1;
		text-align: left;
	}
</style>