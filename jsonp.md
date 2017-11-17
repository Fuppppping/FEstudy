# JSONP 详解


## JSON

在JavaScript中 我们通常使用 Ajax 来进行数据的交换，数据的交换类型通常使用[JSON](https://zh.wikipedia.org/wiki/JSON)    
JSON 作为JavaScript中重要的数据格式，已经独立于JavaScript 很多编程语言都支持了JSON数据类型的解析(如python，GO等)
    
JSON有两种结构，分别为对象
```json
var person = {
        "Name":"CLX",
        "Age": 20
    }
```

数组
```json
var members =[
        {
            "Name":"CLX",
            "Age" : 20
        },
        {
            "Name":"ZhaiGe",
            "Age" : 20
        }
    ]
```


JSON 用{}来描述一组*不同*类型的*无序*键值对集合，用[]来描述一组*相同*类型的*有序*数据集合。子项使用英文逗号进行分隔。键值使用：分隔(键名加双引号)

tips：JSON内部数据中 字符串也必须用双引号

## JSONP

### 为何使用JSONP    

1.Ajax无法跨域无权限访问(但通过服务端代理可以)
2.script/img/iframe等标签(具有src属性)可以跨域，页面调用js也可跨域，为了跨域进行数据交互，我们需要利用这一特性进行数据交互即JSONP
----
### JSONP原理    
由于src属性不受同源政策限制，所以我们可以通过向页面添加script标签来完成数据交互，script标签不止可以调用js文件，也可调用php等动态资源，后台被访问后返回一个函数调用的字符串，在前端script标签内该字符串就变为一个js的函数调用，实参为我们所需要的数据。

但也因为以上原理，jsonp只可以进行GET而不能进行POST因为其参数已经显示生成在URL中。其次动态生成script标签本身就是一种注入行为，需要防止其他别有用心的人通过JSONP的方式进行注入，带来安全隐患。

代码实现：
```js
<script>// 实现回调函数，这里没有了 jQuery 的封装，必须手动指定并实现
var dosomething = function(data){console.log(data);};

// 提供 JSONP 服务的 URL 地址，查询字符串中加入 callback 指定回调函数
var url = "tonghuashuo.github.io/test/jsonp.txt?callback=docomething";

// 创建 <script> 标签，设置其 src 属性
var script = document.createEle1ment('script');
  script.setAttribute('src', url);

  // 把 <script> 标签加入 <body> 尾部，此时调用开始。
  document.getElementsByTagName('body')[0].appendChild(script);

  // 因为目标 URL 是一个后台脚本，访问后会被执行，返回的 JSON 被包裹在回调函数中以字符串的形式被返回。
  // 返回的字符串放入 <script> 中就成为了一个普通的函数调用，执行回调函数，返回的 JSON 数据作为实参被传给了回调函数。
</script>
```

tips：以上引用其他博客的代码内容:)