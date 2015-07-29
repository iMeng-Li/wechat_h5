
# 谈谈基于微信的HTML5开发的实践
------------------------

2013年微信的公众号火了一把，市面突飞出现了喜欢小公司都在弄基于公众号第三方接入开发。
我也不例外，也跑在这条队列中来；我当时是处于5个人的小团队里，不用说了，最后散火了~~~
那是我大三赶着刚考完期末考试，第二天就出现在老板的身边，啊哈哈哈~~~
那是13年的6月底，我们已经开始在弄需求和设计了，而我还要弄另一个小项目，就是为电信弄个高铁网关广告H5页面。

至于那个公众号第三方接入的项目的开始是在7月初开始，我们的速度还是很快的，为了这我还要求大家星期六加班呢（后来很后悔这件事情，那时实在是太年轻了）。

一个月的时间，我们的第一个版本出来了，HTML5的模板只有两个。已经很不错了，当时被很多看好。那股干劲一直燃烧.....

后来我们搬到了广州信息港去了~~

再至于后来，我就不说了，我在第四个月后离开了。

公司在我离开第三个月就玩完了（也就是14年春天来临之际死亡了）~~~老板不给力。

###关于单页面修改title一事

先道出问题：当页面载入时，微信顶部 title 获取并显示，之后怎么用JS修改 title ，无效。

这个是针对IOS出现的 hack ，Android是有效的。
貌似是IOS的webview没有监听titlechange事件。
目前的处理就是使用重新载入页面的形式来唤醒页面的title有关事件。

这里使用在页面重载了个iframe的形式：

```javascript
 //解决iOS设备修改title不起作用的bug
  function set_title(title) {
      document.title = title;
      if (!ua.is_idevice) return;
      //随便访问iframe访问一个网页，title就可以刷新了
      var url = 'http://xxx.html';
      var dom_id = 'refresh-ios-title-iframe';
      var dom = $('#' + dom_id);
      if (dom.length == 0) {
          dom = $('<iframe style="width:0px;height:0px;display:none" id="' + dom_id + '" src="' + url + '" />');
          $("body").append(dom);
      } else {
          dom.attr('src', url);
      }
  }
```

###iOS Safari 中绑定点击事件失效的解决办法

因为项目里的HTML是使用了模板引擎塞进去的，固然统一的事件会用绑定，也可在模板引擎后面添加。
我这里使用了全局事件绑定：
```javascript
// 是否为系统分发礼包
    is_system_activity ? to_system_activity() : distribute_activity();

    $("body").on("touchstart", ".explain", function() {
        if (_hmt) {
            _hmt.push(["_trackEvent", "点击次数", "查看游戏规则"])
        }
        $("#game-info").show();
    })
```
我这里使用了.on()事件，在IOS测试的时候，无效；
我在想是不是.on事件太新了，啊哈哈哈哈啊哈~~~固然不是啦~！！！
我就直接使用了.bind()事件来绑定，测试没问题。

那么现在问题来了，到底为什么会这样呢？

######问题描述
当使用委托给一个元素添加click事件时，如果事件是委托到 document 或 body 上，并且委托的元素是默认不可点击的（如 div, span 等），此时 click 事件会失效。

######解决办法
解决办法有 4 种可供选择：

​将 click 事件直接绑定到目标​元素（​​即 .target）上
将目标​元素换成 <a> 或者 button 等可点击的​元素
​将 click 事件委托到​​​​​非 document 或 body 的​​父级元素上
​给​目标元素加一条样式规则 cursor: pointer;
​推荐后两种。从解决办法来看，​推测在 safari 中，不可点击的元素的点击事件不会冒泡到父级元素。通过添加 cursor: pointer 使得元素变成了可点击的了。

来着：http://happycoder.net/solve-ios-safari-click-event-bug/


###input的一些注意的地方





###IOS的web设置了body height:100%;注意地方







