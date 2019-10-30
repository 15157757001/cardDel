# cardDel

卡片删除效果(仿探探)

# 说明
接口挂了的话，用github的版本

插件设计: 插件不仅仅可以实现仿探探的效果，还能通过参数实现卡片不同的动画效果的变化。

基本模板里提供了简单的动画效果。


## 使用方式

**在index.js中**  

~~~
import clCardDel from "@/components/clCardDel/clCardDel";
mixins:[clCardDel]
~~~

## OBJECT参数说明

| 参数 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| number | Number | 2 | 存在的卡片数量 |
| moveRotate | Object | { x:0,y:0 } | 设置位移图片旋转角度距离  card中心点 - 指向坐标 |
| delMoveD | Number | screenHeight*2 | 删除移动距离 |
| touchMoveD | Number | 100 | card移动距离 card移动距离/touchMoveD = 其他card变化比率 |
| rotate | Number | 0 | 第2张卡片旋转角度 |
| scale | Object | { x:1,y:1 } | 第2张卡片缩放 |
| skew | Object | { x:0,y:0 } | 第2张卡片倾斜 |
| translate | Object | { x:0,y:0 } | 第2张卡片位移 |
| opacity | Number | 1 | 第2张卡片透明度 |

## 事件

| 事件名 | 说明 |
| ---  | --- |
| init | 设置初始参数 |
| getData | 获取数据 |
| moveJudge(x,y,ratio) | 触摸中判断 |
| endJudge(x,y) | 触摸结束判断 |
| _back | 执行回退动画 |
| _del | 执行删除动画 |

## 如果觉得插件不错，麻烦给个好评
