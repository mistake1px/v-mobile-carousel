# v-mobile-carousel

一款用于移动端的轮播图vue插件

## 演示

![demo示例图](https://github.com/mistake1px/v-mobile-carousel/blob/master/examples/assets/carousel.gif)

## 安装

``` bash
yarn add v-mobile-carousel
Or
npm i v-mobile-carousel
```

## 使用

``` bash
import Carousel from 'v-mobile-carousel'
export default {
  components: { Carousel }
}
```

## api

### props

|参数 | 取值 | 必选 |说明
|---|----|---|---|
|list| Array| 必选|用于显示的图片列表[{src: ''}, ...]|
|interval| Number| 非必选|自动播放，一次循环的时长|
|threshold| Number| 非必选|手动滑动时，超过该值才会滑动|
|auto| Boolean| 非必选|是否默认进入循环，默认为true|

### events

|名称 | 参数 | 说明 |
|---|----|---|---|
|@index-change| Number| 当内部图片变化时，透传当前处于active的数组索引，该索引默认应为0|

## License (MIT)
