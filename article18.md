# 鸿蒙Next开发教程--在鸿蒙中开发图表（ECharts）
今天要分享的教程是如何在鸿蒙开发中使用图表，说到图表不得不提ECharts，它依旧是目前为止功能最丰富、兼容性最高的图表库。所以我依然选择使用ECharts。
![image](https://github.com/user-attachments/assets/872ac71c-99d5-4cd8-aeba-c36915955248)


在其他平台使用过ECharts的友友都知道，ECharts的使用流程是先使用html引入ECharts库，然后在界面中使用webview将html加载出来，最后将图表数据注入就可以了。在鸿蒙中依然不例外。

ECharts库可以到官网定制下载，下载后使用html引入图表：
```

<body>
<div id="chart"></div>
</body>
<script>
    const container = document.getElementById('chart')
    function init(width, height) {
        container.style.width = width;
        container.style.height = height;
        window.myChart = window.echarts.init(container);
        window.myChart.on('click', function(params) {
          window.ohosCallNative.callNative('click', {
            componentType: params.componentType,
            seriesType: params.seriesType,
            seriesIndex: params.seriesIndex,
            seriesName: params.seriesName,
            name: params.name,
            dataIndex: params.dataIndex,
            dataType: params.dataType,
            value: params.value,
            color: params.color,
          });
        });
    }
</script>
```
然后在ArkTS中加载html：

```
Web({ src: $rawfile('echarts.html'), controller: this.controller })
      .width('100%')
      .height('100%')
      .javaScriptAccess(true)
      .backgroundColor(this.webBackgroundColor)
      .onPageBegin(() => {
       
      })
      .onPageEnd(e => {
        this.init(this.canvasWidth, this.canvasHeight);
        this.setOption(this.option);
        if (this.onLoaded) {
          this.onLoaded(this);
        }
      })
```
关于向图表中注入数据，我们使用Web组件的runJavaScript方法来实现：

```
option: EChartsOption = {
    title: {
      text: 'ECharts 入门示例'
    },
    tooltip: {},
    legend: {
      data: ['销量']
    },
    xAxis: {
      data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
    },
    yAxis: {},
    series: [
      {
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
      }
    ]
  };


this.controller.runJavaScript(`myChart.setOption(${JSON.stringify(option)})`);
```
