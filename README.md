
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













