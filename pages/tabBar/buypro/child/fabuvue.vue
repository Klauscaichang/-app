<template>
	<view class="container-fabu">

		<view class="form-box">
			<view class="uni-flex item-box">
				<view class="uni-label-box">
					<image class="img-icon" src="/static/img/huozhu/leibie.png"></image>
					<text class="text bitian">货物类别</text>
				</view>
				<view class="input-box">
					<view class="input" :class="protype.value?'':'place'">
						{{protype.value ? protype.value : protype.label }}
					</view>
					<uni-icons class="icon" type="arrowdown"></uni-icons>
				</view>
			</view>
			<view class="uni-flex item-box">
				<view class="uni-label-box">
					<image class="img-icon" src="/static/img/huozhu/shulian.png"></image>
					<text class="text bitian">货物数量(吨)</text>
				</view>
				<view class="input-box">
					<input class="uni-input" v-model="numberleng" type="number" placeholder="请输入货物数量（可预估）" />
				</view>
			</view>
			<view class="uni-flex item-box">
				<view class="uni-label-box">
					<image class="img-icon" src="/static/img/huozhu/shoujia.png"></image>
					<text class="text">售价</text>
				</view>
				<view class="input-box">
					<input class="uni-input" v-model="price" type="number" placeholder="请输入货物数量（可预估）" />
				</view>
			</view>
			<view class="uni-flex item-box">
				<view class="uni-label-box">
					<image class="img-icon" src="/static/img/huozhu/dizhi.png"></image>
					<text class="text bitian">货物地址</text>
				</view>
				<view class="input-box" @tap="chooseCity">
					<view class="input " :class="region.cityCode?'':'place'">{{region.label}}</view>
					<uni-icons class="icon" type="arrowdown"></uni-icons>
				</view>
			</view>
			<view class="uni-flex item-box">
				<view class="uni-label-box">
					<image class="img-icon" src="/static/img/huozhu/lianxiren.png"></image>
					<text class="text bitian">联系人</text>
				</view>
				<view class="input-box">
					<input class="uni-input" v-model="contentname" maxlength="8" type="text" placeholder="请填写联系人称呼" />
				</view>
			</view>
			<view class="uni-flex item-box">
				<view class="uni-label-box">
					<image class="img-icon" src="/static/img/huozhu/dianhua.png"></image>
					<text class="text bitian">联系电话</text>
				</view>
				<view class="input-box">
					<input class="uni-input" v-model="contentphone" maxlength="11" type="number" placeholder="请填写联系人电话号码" />
				</view>
			</view>
			<view class="uni-flex item-box">
				<view class="uni-label-box">
					<image class="img-icon" src="/static/img/huozhu/pipeishu.png"></image>
					<text class="text">允许匹配数</text>
				</view>
				<view class="input-box">
					<input class="uni-input" v-model="pipeinum" maxlength="2" type="number" placeholder="如不输入，系统默认无限量" />
				</view>
			</view>


			<view class="uni-list list-pd">
				<view class="uni-list-cell cell-pd">
					<view class="uni-uploader">
						<view class="uni-uploader-head">
							<view class="uni-uploader-title bitian">上传图片</view>
							<view class="uni-uploader-info">{{imageList.length}}/9</view>
						</view>
						<view class="uni-uploader-body">
							<view class="uni-uploader__files">
								<block v-for="(image,index) in imageList" :key="index">
									<view class="uni-uploader__file">
										<image class="uni-uploader__img" :src="image" :data-src="image" @tap="previewImage"></image>
									</view>
								</block>
								<view class="uni-uploader__input-box">
									<view class="uni-uploader__input" @tap="chooseImage"></view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>

		</view>

		<view class="fo-box">
			<view class="primary-btn" @tap="fabu">
				发布审核
			</view>
		</view>

		<view class="point">
			<icon type="warn" size="12" />
			<text class="text">中途退出系统将不保留此次编辑</text>
		</view>

		<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" :pickerValueDefault="cityPickerValue"
		 @onConfirm="onConfirm"></mpvue-city-picker>

	</view>
</template>

<script>
	var sourceType = [
		['camera'],
		['album'],
		['camera', 'album']
	];
	var sizeType = [
		['compressed'],
		['original'],
		['compressed', 'original']
	]
	import permision from "@/common/permission.js"
	import mpvueCityPicker from '@/components/mpvue-citypicker/mpvueCityPicker.vue'
	import uniIcons from '@/components/uni-icons/uni-icons.vue'
	export default {
		components: {
			uniIcons,
			mpvueCityPicker
		},
		data() {
			return {
				themeColor: '#007AFF',
				protype: {
					label: "请选择货物类别",
					value: "",
				},

				region: {
					label: "请点击选择地址",
					value: [],
					cityCode: ""
				},
				cityPickerValue: [0, 0, 1],
				numberleng: "",
				price: "",
				contentname: '',
				contentphone: '',
				pipeinum: "",

				sourceType: ['拍照', '相册', '拍照或相册'],
				sourceTypeIndex: 2,
				imageList: [],
				countIndex: 8,
				count: [1, 2, 3, 4, 5, 6, 7, 8, 9]
			}
		},
		computed: {

		},
		methods: {
			fabu() {
				uni.navigateTo({
					url: "/pages/other/fabusuccess"
				})
			},
			async checkPermission(code) {
				let type = code ? code - 1 : this.sourceTypeIndex;
				let status = permision.isIOS ? await permision.requestIOS(sourceType[type][0]) :
					await permision.requestAndroid(type === 0 ? 'android.permission.CAMERA' :
						'android.permission.READ_EXTERNAL_STORAGE');

				if (status === null || status === 1) {
					status = 1;
				} else {
					uni.showModal({
						content: "没有开启权限",
						confirmText: "设置",
						success: function(res) {
							if (res.confirm) {
								permision.gotoAppSetting();
							}
						}
					})
				}

				return status;
			},
			isFullImg: function() {
				return new Promise((res) => {
					uni.showModal({
						content: "已经有9张图片了,是否清空现有图片？",
						success: (e) => {
							if (e.confirm) {
								this.imageList = [];
								res(true);
							} else {
								res(false)
							}
						},
						fail: () => {
							res(false)
						}
					})
				})
			},
			chooseImage: async function() {
				// #ifdef APP-PLUS
				// TODO 选择相机或相册时 需要弹出actionsheet，目前无法获得是相机还是相册，在失败回调中处理
				if (this.sourceTypeIndex !== 2) {
					let status = await this.checkPermission();
					if (status !== 1) {
						return;
					}
				}
				// #endif

				if (this.imageList.length === 9) {
					let isContinue = await this.isFullImg();
					if (!isContinue) {
						return;
					}
				}
				uni.chooseImage({
					sourceType: sourceType[this.sourceTypeIndex],
					sizeType: sizeType[this.sizeTypeIndex],
					count: this.imageList.length + this.count[this.countIndex] > 9 ? 9 - this.imageList.length : this.count[this.countIndex],
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					},
					fail: (err) => {
						// #ifdef APP-PLUS
						if (err['code'] && err.code !== 0 && this.sourceTypeIndex === 2) {
							this.checkPermission(err.code);
						}
						// #endif
						// #ifdef MP
						uni.getSetting({
							success: (res) => {
								let authStatus = false;
								switch (this.sourceTypeIndex) {
									case 0:
										authStatus = res.authSetting['scope.camera'];
										break;
									case 1:
										authStatus = res.authSetting['scope.album'];
										break;
									case 2:
										authStatus = res.authSetting['scope.album'] && res.authSetting['scope.camera'];
										break;
									default:
										break;
								}
								if (!authStatus) {
									uni.showModal({
										title: '授权失败',
										content: 'Hello uni-app需要从您的相机或相册获取图片，请在设置界面打开相关权限',
										success: (res) => {
											if (res.confirm) {
												uni.openSetting()
											}
										}
									})
								}
							}
						})
						// #endif
					}
				})
			},
			previewImage(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			onCancel(e) {
				console.log(e)
			},
			chooseCity() {
				this.$refs.mpvueCityPicker.show()
			},
			onConfirm(e) {
				this.region = e;
				this.cityPickerValue = e.value;
			},
		}
	}
</script>

<style lang="scss" scoped>
	.container-fabu {
		padding: 40upx 20upx 0upx;
		.fo-box {
			display: flex;
			justify-content: center;
			margin-top: 20upx;
			margin-bottom: 20upx;
			.primary-btn {
				width: 326upx;
				height: 98upx;
				line-height: 98upx;
				text-align: center;
				background-color: #18C02C;
				border-radius: 8upx;
				color: #fff;
				font-size: 38upx;
			}
		}
		.uni-list:before {
			height: 0;
		}

		.uni-list:after {
			height: 0;
		}

		.uni-uploader__input-box,
		.uni-uploader__file,
		.uni-uploader__img {
			width: 147upx;
			height: 147upx;
		}

	}

	.form-box {
		.item-box {
			margin-bottom: 12upx;

			.uni-label-box {
				flex: 2;
				display: flex;
				align-items: center;

				.img-icon {
					width: 40upx;
					height: 40upx;
					margin-right: 10upx;
				}

				.text {
					font-size: 33upx;
				}
			}

			.input-box {
				flex: 3;
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 1upx 20upx;
				height: 70upx;
				border: 1px solid #575757;
				border-radius: 6px;

				.input {
					&.place {
						color: #575757;
					}
				}

				.uni-input {
					padding: 0;
				}
			}
		}
	}

	.point {
		margin-top: 30upx;
		color: #555555;
		display: flex;
		justify-content: center;
		align-items: center;

		.text {
			margin-left: 10upx;
		}
	}
</style>
