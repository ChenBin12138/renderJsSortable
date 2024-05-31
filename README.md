## 关于兼容性
此插件只适用于APP项目（Android/Ios）、Web项目，不支持小程序！！关于renderjs说明

## 关于运行
1. 下载示例项目
2. 安装依赖 ```npm install sortablejs```
3. 运行至浏览器

## 使用示例

```
<sortable-renderjs :options="{}" @onListChange="verticalListChange">
	<view class="vertical item-ul">
		<view class="item-li" v-for="(str,i) of list" :key="i" :data-id="str">{{str}}</view>
	</view>
</sortable-renderjs>

```

## 参数

options 参照官网配置信息 http://www.sortablejs.com/

## 事件
@onListChange 列表发生改变，对应sortablejs的onAdd/onRemove/onMove事件发生改变都会回掉