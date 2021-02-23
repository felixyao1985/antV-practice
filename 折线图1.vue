/*
 折线图：同线，不同颜色及标点
*/

import F2 from '@antv/f2';
import _ from 'lodash';

function randomString(len) {
　　len = len || 32;
　　var $chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';
　　var maxPos = $chars.length;
　　var str = '';
　　for (let i = 0; i < len; i++) {
　　　　str += $chars.charAt(Math.floor(Math.random() * maxPos));
　　}
　　return str;
}


    var data = [{
      day: '周一',
      value: 300,
    }, {
      day: '周二',
      value: 400,
    }, {
      day: '周三',
      value: 350,
    }, {
      day: '周四',
      value: 500,
    }, {
      day: '周五',
      value: 490,
    }, {
      day: '周六',
      value: 600,
    }, {
      day: '周日',
      value: 900,
    }];



    var type = randomString(8)

    var trend = 1
    if (data[0].value > 400) {
	    trend = 0
    }

    var newData = []
    for(var key in data){ 

      var item = data[key]
      var nowtrend = 1
      if (item.value > 400) {
        nowtrend = 0
      }
    
      if (nowtrend != trend) {
        trend = nowtrend
        type  = randomString(8)

        let temp = data[(key-1)]
       
        newData.push({
          day:temp.day,
          value:0,
          type:trend+type,
          trend:temp.trend
        })
      }

      item.type = trend+type
      item.trend = trend
      newData.push(item)
    };


    const chart = new F2.Chart({
      id: 'container',
      animate: false,
      pixelRatio: window.devicePixelRatio
    });
    chart.source(newData);
    chart.axis('year');

    chart.line().position('day*value').adjust('stack')
      .color('type', function(val) {
        if (val[0] == 1) {
          return '';
        } else{
          return 'red';
        }
    });

    chart.point().position('day*value').adjust('stack')
      .color('trend', function(val) {
        console.log(val)
        if (val == 1) {
          return 'blue';
        } else{
          return 'red';
        }
    });

    chart.legend(false)

    chart.render();


/*
秀逗了 后来发现AntV可以用辅助过滤区域。吐槽一下文档好乱

如果引入的是 @antv/f2，那么默认不包含 Guide.RegionFilter，如您要使用这个辅助类，那么需要将其引入，方法如下：
```

import f2 from '@antv/f2/lib/index';
require('@antv/f2/lib/component/guide/region-filter');

chart.guide().regionFilter({ // 绘制区域过滤
  start: [ 'min', 95 ],
  end: [ 'max', 'max' ],
  color: '#FF4D4F'
});

```
*/