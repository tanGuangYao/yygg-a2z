<template>
	<view>
		<view class="left-box">
			<view class="" id="left_box" @mouseleave.stop @tap.stop.prevent="goToIndex" @click.stop.prevent="goToIndex"
				@touchmove.stop.prevent="goToIndex" @mousemove.stop.prevent @touchend.stop>
				<view class="left-box-item" v-for="(item,index) in index_array" :key="index">
					{{item}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "yygg-a2z",
		props: {
			comList: {
				type: Array,
				default: () => {
					return new Array()
				}
			}
		},
		data() {
			return {
				duration: 500,
				scrollTop: 100,
				indexList: ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S",
					"T", "U",
					"V", "W", "X", "Y", "Z"
				],
				new_list_arr: [],
				index_array: [],
				move_index: '',
				max_height: 0,
				max_top: 0,
			};
		},
		methods: {
			getDataList() {
				let list_arr = []
				let index_array = []
				this.comList.forEach((item, index) => {
					if (!list_arr[item.first]) {
						list_arr[item.first] = []
						index_array.push(item.first)
					}
					list_arr[item.first].push(item)
				})
				this.index_array = index_array
				let new_list_arr = []
				for (let ind in list_arr) {
					let index = ind; //对象中的每一项的键名
					let value = list_arr[ind]; //对象中的每一项的值

					new_list_arr.push({
						'letter': index,
						'data': value
					})
				}
				this.new_list_arr = new_list_arr

				this.$emit('new_list_arr', new_list_arr)

				this.$nextTick(() => {
					uni.createSelectorQuery().select("#left_box").boundingClientRect(
						data => { //目标位置的节点：类class或者id
							if (!data) return
							this.max_height = data.height
							this.max_top = data.top
						}).exec();
				})
			},

			goToIndex(num1) {
				let yy = 0
				switch (num1.type) {
					case 'touchmove':
						yy = num1.touches[0].clientY
						break;
					case 'click':
						yy = num1.detail.y
						break;
					case 'tap':
						yy = num1.touches[0].clientY
						break;
				}
				if (yy == 0) return
				// this.max_top  渲染完数据后 组件距离顶部的距离
				// this.max_height  渲染完数据后 组件的高度
				// this.index_array  索引列表  A-Z
				let n_yy = yy - this.max_top
				let max_h = this.max_height
				if (n_yy < max_h && n_yy > 0) {
					let i = Math.round(n_yy * (this.index_array.length / max_h));
					if (i == 0) {
						i = 1
					}
					let num = this.index_array[i - 1]
					this.move_index = num
					uni.showToast({
						title: num,
						icon: "none"
					}, 300)
					uni.createSelectorQuery().select("#index1_" + num).boundingClientRect(data => { //目标位置的节点：类class或者id
						if (!data) return
						uni.createSelectorQuery().select("#home").boundingClientRect(
							res => { //最外层盒子的节点：类class或者id
								uni.pageScrollTo({
									duration: 100, //过渡时间
									scrollTop: data.top - res.top - 200,
								})
							}).exec()
					}).exec();
				}
			},
		}
	}
</script>

<style lang="scss">
	.left-box {
		width: 50rpx;
		height: 936rpx;
		position: fixed;
		top: 35%;
		right: 0;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		.left-box-item {
			width: 25rpx;
			margin: 2rpx 0 2rpx 23rpx;
			font-size: 20rpx;
			// background-color: #FFCB05;
			font-weight: 600;
			color: #181818;
			display: flex;
			align-items: center;
			justify-content: center;
		}
	}
</style>
