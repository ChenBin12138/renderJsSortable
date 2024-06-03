<template>
	<view :prop="options" :change:prop="Sortable.createSortableJs">
		<slot />
	</view>
</template>

<script>
	export default {
		props: {
			// SotableJs Options
			options: {
				type: Object,
				default: function() {
					return {}
				}
			}
		},
		methods: {
			onListChange(list) {
				this.$emit('onListChange', list);
				this.$emit('changeList', list);
			}
		}
	}
</script>

<script module="Sortable" lang="renderjs">
	import Sortable from 'sortablejs';

	export default {
		data() {
			return {
				sortable: null
			}
		},
		methods: {
			async createSortableJs(options = {}) {

				await this.$nextTick();
				
				
				if (this.sortable) {
					Object.keys(options).forEach(k => {
						this.sortable.option(k, options[k]);
					})
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

					// sortable event
					eventNames.forEach(eventName => {
						const optCall = options[eventName];
						sortableOptions[eventName] = (evt) => {
							// source options call 
							optCall && optCall(evt);

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

			}
		}
	}
</script>