### 关于兼容性

此插件只适用于APP项目（Android/Ios）、Web项目，不支持小程序！！[关于renderjs说明](https://uniapp.dcloud.io/frame?id=renderjs)

### 说明

1.  请先安装sortablejs，在项目根目录执行 ```npm install sortablejs  ```
2.  有示例项目请先运行示例项目了解使用方法。
2.  sortablejs更多参数配置参考[sortablejs官网](http://www.sortablejs.com/index.html)

### 示例代码

```html
<sortable-renderjs domId="horizontal" @changeList="horizontalListChange">
    <view id="horizontal" class="horizontal item-ul">
        <!-- data-id数据将会在排序改变后在事件changeList返回 -->
        <view class="item-li" v-for="(item,i) of list" :key="i" :data-id="item">{{item}}</view>
    </view>
</sortable-renderjs>
```

## 传入参数

| 属性名  | 是否必填 | 值类型 | 说明                                           |
| ------- | -------- | ------ | ---------------------------------------------- |
| domId   | 是       | String | 如上示例代码需拖动元素的父元素Id（延迟调用不要直接写死）  |
| options | 否       | Object | 参照http://www.sortablejs.com/官网进行参数配置 |

## 事件

| 事件名     | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| changeList | 在排序发生改变后触发，会将子元素上的data-id数据以数组形式返回 |

