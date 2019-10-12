---
title: component-taro-echarts
date: 2019-10-12
tags: 
    - Taro
categories: Taro
keywords: [Taro]
description: component-taro-echarts
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://i.loli.net/2019/10/11/RFuNLECdtQo5bHw.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


### [Taro🔨] 为了兼容小程序 Canvas，将ECharts官方提供的小程序组件转化成Taro代码组件。
<br>


### 一、[资料推荐]ECharts官方
为了兼容小程序 Canvas，我们提供了一个小程序的组件，用这种方式可以方便地使用 ECharts。

首先，下载 GitHub 上的 [ecomfe/echarts-for-weixin](https://github.com/ecomfe/echarts-for-weixin) 项目。
<br>


### 二、[资料推荐]Taro 物料市场 `ecahrts12`
[npm ecahrts12](https://taro-ext.jd.com/plugin/view/5d439e9b08267b4198ad0c63)
<br>


### 三、[GitHub 文档下载地址](https://github.com/LOUSANPANG/component-taro-echarts)
<br>


### 四、文档使用
* 下载该文件，将文件内的 `BaseEcharts` `EcCanvas` 拷入到`components`项目中。
* 在对应页面组件引入 `BaseEcharts` 、 定义组件配置参数即可。
```
// pages/index/index.jsx

import Echart from '../../components/BaseEcharts/BaseEcharts'

state = {
    option: {
        //...
    }
}

render() {
    let {
        option
    } = this.state

    return (
        <View className='index'>
            <Echart 
                option={option} 
                style='height: 50vh'

            />
        </View>
    );
}
```
<br>


### 五、配置参数

| 属性名 | 说明                                                               | 必选 |
| ------ | ------------------------------------------------------------------ | ---- |
| option | 同 echarts 的 [option 可参照echarts默认官方示例](https://github.com/ecomfe/echarts-for-weixin) 配置 | 是   |
| onInit | 初始化回调函数，参数为 chart ，可以通过 chart 做事件绑定等操作     | 否   |
| style  | 自定义画布大小，默认 100vh 的 height                        | 否   |
| echartsType | echarts类型为地图                   | 否   |
<br>


### 六、echarts类型为地图的情况
需要手动在`BaseEcharts/BaseEcharts`中开启。

并且引入你用到的`geojson 文件`

请参照 [echarts官方示例](https://github.com/ecomfe/echarts-for-weixin/tree/master/pages/map)
```
// conponents/BaseEcharts/BaseEcharts

chart = echarts.init(canvas, null, {
    width,
    height
});
canvas.setChart(chart);

// 地图类型 geoJson json文件 (需要开启，并引入地图json文件)
// if (this.props.echartsType) echarts.registerMap('map', geoJson);

chart.setOption(option);
this.props.onInit && this.props.onInit(chart);
this.chart = chart;
return chart;
```
<br>


### 七、实例图片
![map.png](https://i.loli.net/2019/10/11/RFuNLECdtQo5bHw.png)
![pie.png](https://i.loli.net/2019/10/11/mI6O7lTtufWB4Zn.png)
<br>


### 八、支持情况

| 微信小程序 | h5  |
| ---------- | --- |
| ✅         | ✅  |
<br>

