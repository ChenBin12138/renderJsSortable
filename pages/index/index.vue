<template>
	<view>
		<view class="prompt">子元素 data-id 属性在排序发生改变触发 onListChange 事件时以数组形式返回，data-id请传唯一标识不要传对象</view>

		<view class="tab_ul">
			<view class="tab_li" v-for="(tab,i) of tabs" :key="tab.type" @click="currentType = tab.type" :class="{
				active: currentType == tab.type
			}">
				{{ tab.label }}
			</view>
		</view>

		<template v-if="currentType == 0">
			<sortable-renderjs @onListChange="horizontalListChange" :options="{
			animation: 1000
		}">
				<view class="horizontal item-ul">
					<view class="item-li" v-for="(str,i) of list" :key="i" :data-id="str">{{str}}</view>
				</view>
			</sortable-renderjs>

		</template>

		<template v-if="currentType ==1">
			<sortable-renderjs :options="{}" @onListChange="verticalListChange">
				<view class="vertical item-ul">
					<view class="item-li" v-for="(str,i) of list" :key="i" :data-id="str">{{str}}</view>
				</view>
			</sortable-renderjs>
		</template>

		<template v-if="currentType == 3">

			<view style="color: red;font-size: 30rpx;padding: 10rpx;">设置options中group命名一致即可</view>

			<view class="sort_group">
				<sortable-renderjs :options="{
					group: 'group11'
				}" @onListChange="groupListChange($event,'左边')">
					<view class="horizontal item-ul">
						<view class="item-li" v-for="(str, i) of list1" :key="i" :data-id="str">{{str}}</view>
					</view>
				</sortable-renderjs>

				<sortable-renderjs :options="{
					group: 'group11'
				}" @onListChange="groupListChange($event,'右边')">
					<view class="horizontal item-ul">
						<view class="item-li" v-for="(str, i) of list2" :key="i" :data-id="str">{{str}}</view>
					</view>
				</sortable-renderjs>

			</view>

		</template>
	</view>
</template>

<script>
	import sortableRenderjs from '@/uni_modules/cms-sortable-renderjs/components/cms-sortable-renderjs/cms-sortable-renderjs';

	export default {
		components: {
			sortableRenderjs
		},
		data() {
			return {
				currentType: 0,
				tabs: [{
						label: '横向拖拽示例',
						type: 0
					},
					{
						label: '竖向拖拽示例',
						type: 1
					},

					{
						label: '相互拖拽示例',
						type: 3
					}
				],
				list: [],
				list1: [],
				list2: [],
			}
		},
		mounted() {
			this.getData();
		},
		methods: {
			getData() {
				for (let i = 0; i < 10; i++) {
					let str = this.getRandomChineseWord();
					this.list1.push(`${i}:${str}`);
				}
				for (let i = 0; i < 10; i++) {
					let str = this.getRandomChineseWord();
					this.list2.push(`${i}:${str}`);
				}

				this.setList();

				setTimeout(() => {
					this.setList();
				}, 5000)
			},
			setList() {
				for (let i = 0; i < 10; i++) {
					let str = this.getRandomChineseWord();
					this.list.push(`${i}:${str}`);
				}
			},
			// 随机获取中文
			getRandomChineseWord() {
				var str = "";
				let count = Math.floor(Math.random() * 10);
				count = Math.max(count, 3);
				for (var i = 0; i < count; i++) {
					let _randomUniCode = Math.floor(Math.random() * (40870 - 19968) + 19968).toString(16);
					let _rsl = '';
					eval("_rsl=" + '"\\u' + _randomUniCode + '"');
					str += _rsl;
				}
				return str;
			},
			groupListChange(list, type) {
				uni.showToast({
					title: '列表发生改变,请在控制台查看。',
					icon: 'none'
				})
				console.log('groupListChange:' + type, list)
			},
			horizontalListChange(list) {
				uni.showToast({
					title: '列表发生改变,请在控制台查看。',
					icon: 'none'
				})
				console.log('horizontalListChange changelist :: ', list);
			},
			verticalListChange(list) {
				uni.showToast({
					title: '列表发生改变,请在控制台查看。',
					icon: 'none'
				})
				console.log('verticalListChange changelist :: ', list);
			}
		}
	}
</script>

<style scoped lang="scss">
	.prompt {
		color: red;
		display: flex;
		align-items: center;
		padding: 15rpx;
		word-break: break-all;
		font-size: 30rpx
	}

	.item-ul {
		.item-li {
			padding: 5rpx;
			margin: 10rpx;
			border: 1rpx dashed #999;
		}
	}

	.horizontal {
		display: flex;
		flex-wrap: wrap;
	}

	.title {
		margin: 50rpx 0;
		font-size: 38rpx;
		font-weight: bold;
	}

	.tab_ul {
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: 24rpx;
		border-top: 1px solid #e2e2e2;
		border-bottom: 1px solid #e2e2e2;
		padding: 15rpx 0;

		.tab_li {
			flex: 1;
			padding: 0 10rpx;
			text-align: center;
		}


		.tab_li+.tab_li {
			border-left: 1px solid #e2e2e2;
		}

		.tab_li.active {
			color: #0842a0;
		}
	}

	.sort_group {
		display: flex;
		align-items: center;

		>view {
			width: 50%;
			min-height: 600rpx;
		}
	}
</style>