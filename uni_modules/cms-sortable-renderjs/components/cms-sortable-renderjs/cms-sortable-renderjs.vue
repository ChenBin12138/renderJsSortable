<template>
	<view :prop="renderJsOptions" :change:prop="Sortable.create">
		<slot />
	</view>
</template>

<script>
	export default {
		props: {
			// 对Sortable参数配置可以写在此处或传递进入（App暂不可用）
			options: {
				type: Object,
				default: function() {
					return {}
				}
			}
		},
		methods: {
			// sortablejs的所有事件
			onSortableJsEvent({
				eventName,
				evt
			}) {
				this.$emit(eventName, evt);
			},
			// 列表发生改变
			onListChange(list) {
				this.$emit('onListChange', list);
				// 对老版本事件兼容
				this.$emit('changeList', list);
			}
		},
		computed: {
			// renderjs 貌似只接受一个prop参数 所以将数据打包传递
			renderJsOptions() {
				return {
					options: this.options
				}
			}
		}
	}
</script>

<script module="Sortable" lang="renderjs">
	import Sortable from 'sortablejs';


	// 深克隆
	function deepClone(source) {
		if (!source && typeof source !== 'object') {
			throw new Error('error arguments', 'shallowClone')
		}
		const targetObj = source.constructor === Array ? [] : {}
		for (const keys in source) {
			if (source.hasOwnProperty(keys)) {
				if (source[keys] && typeof source[keys] === 'object') {
					targetObj[keys] = source[keys].constructor === Array ? [] : {}
					targetObj[keys] = deepClone(source[keys])
				} else {
					targetObj[keys] = source[keys]
				}
			}
		}
		return targetObj
	}

	export default {
		data() {
			return {
				sortable: null
			}
		},
		methods: {
			create() {
				this.$nextTick(() => {

					const options = deepClone(this.renderJsOptions.options || {});

					// 避免二次初始化
					if (this.sortable) {
						return;
					}

					const taretNodes = this.$ownerInstance.$el.childNodes
					if (taretNodes.length) {

						const eventNames = [
							'onChoose', 'onStart', 'onEnd', 'onAdd', 'onUpdate', 'onRemove',
							'onFilter', 'onMove', 'onClone'
						];

						const sortableOptions = {
							animation: 150,
						}

						// 事件
						eventNames.forEach(eventName => {
							const optionsCall = options[eventName];
							sortableOptions[eventName] = (evt) => {
								//拖拽时候添加有新的节点的时候发生该事件
								optionsCall && optionsCall(evt);
								if (['onMove', 'onAdd', 'onRemove'].includes(eventName)) {
									this.$ownerInstance.callMethod('onListChange', sortable.toArray());
								}
							}

							delete options[eventName];
						})

						Object.assign(sortableOptions, options);

						const sortable = Sortable.create(taretNodes[0], sortableOptions);
						this.sortable = sortable;
					}

				});
			}
		}
	}
</script>


<style scoped lang="scss">

</style>