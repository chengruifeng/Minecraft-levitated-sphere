### getReleaseInfo 函数

> getReleaseInfo 为一个开发者自定义函数

> 只需要为软件提供固定的Json字符串即可

> 其中的变量以及逻辑完全自定义

> Json数据只需要包含以下:**应用名称、应用版本、下载链接**

示例:
```Json
[
  {
    "version_number": "",
    "change_log": "",
    "assets": [
      {
        "name": "",
        "download_url": ""
      }
    ]
  }
]

```

详解:
```
[
  {
    "version_number": "应用版本",
    "change_log": "更新日志",
    "assets": [
      {
        /*
         * 应用名称后面可以跟随一个版本号
         * 示例:
         * [应用名称]应用版本
         */
        "name": "应用名称",
        "download_url": "下载链接"
      }
    ]
  }
]

```

我们可以尝试使用JavaScript生成一个可用的Json数据:

```JavaScript

/*
 *jsonstring(应用名称,应用版本,下载链接,更新日志)
 *上面传入的每一个值，都是根据你的定义
 *假如我们使用了JSUtils.selNByJsoupXpath函数
 *那么它返回的是一个数组
 *我们在代码中获取数组的数据同样需要改变
 *比如获取数组中的数据:version_array.get()
 *需要注意的是JSUtils.selNByJsoupXpath返回的是一个Java数组，与JavaScript数组有所不同
 *在JavaScript中，获取数组应该是:version_array[]
 *下面以我的数据为例，详细解释
 */

//App_name来自getDefaultName,version_array来自JSUtils.selNByJsoupXpath,url_array来自JSUtils.selNByJsoupXpath,change来自JSUtils.selNByJsoupXpath)
//所以我所有的数组都是Java数组，应该使用.get()方法
function jsonstring(App_name,version_array,url_array,change){
 //我们定义一个空数组 datas ，它就是Json数据外面的中括号[]
  var datas = [];
 //因为每个数据都是一个数组，所以我们需要使用for循环取出数据
 //所以我们必须取一个绝对值的长度，我这里选择了version_array，作为参考数据
 //通过version_array.size我们可以得到它里面包含了多少个数据
  for (var i = 0; i < version_array.size(); i++) {
    //定义第二个大括号
    var data = {};
    //定义assets的中括号，可以理解为数组
    var assets = [];
    //定义assets中的大括号
    var asset = {};
    //我们把应用名称加入到Json数据中,需要注意这里如果出错请修改成
    //asset["name"] = "" + App_name.get(i);
    asset["name"] = "[" + App_name.get(i) + "]" + version_array.get(i);
    //加入下载链接
    asset["download_url"] = "" + url_array.get(i);
    //将assets节点先push出来
    assets.push(asset);
    //版本号，需要注意前面为什么加一个空字符。也就是:""
    //因为使用Xpath爬取的数据里面可能有特殊字符或者是空字符串
    //""就代表着使用Java的toString方法，强行转换成字符串
    //如果有提示Json Type错误，应该注意查看是否有进行强转
    //这也是我经常犯得错误
    data["version_number"] = "" + version_array.get(i);
    //更新日志
    data["change_log"] ="" + change.get(i);
    data["assets"] = assets;
    datas.push(data);
  }
 //最后返回Json数据
  return JSON.stringify(datas);
}

```

getReleaseInfo 返回Json数据的内容解释完毕。

下一步，我们可以通过实例来了解具体的运行方式。

[ 实例解析 >>> ](https://github.com/DUpdateSystem/UpgradeAll-rules/wiki/%E5%AE%9E%E4%BE%8B%E8%A7%A3%E6%9E%90)
