**自定义源的功能实现依赖 [Rhino](https://github.com/mozilla/rhino)**

> 如果你遇到了问题，请查阅 [Rhino 官方文档](https://developer.mozilla.org/zh-CN/docs/Mozilla/Projects/Rhino/Documentation)，其中包括了 官方设计理念和一些你可能感兴趣的重要的细节（[Rhino 语法支持表](https://mozilla.github.io/rhino/compat/engines.html)），同时也欢迎联系我们。

# 示例代码

以 应用汇 软件源为例：

```js

/*
 * 提交须知:
 * 1.提交前先本地调试脚本运行状态
 * 2.提交前查看脚本的调试标记
 * (将Log.v,等调试函数注释)
 * 3.每个脚本增加注释
 * (提高脚本可读性,维护性)
 *
 */
var userAgent =
  "Mozilla/5.0 (X11; Linux x86_64; rv:67.0) Gecko/20100101 Firefox/67.0";

function getReleaseInfo() {
  /*
   * 获取版本号
   * 得到第一个版本号
   * 数据格式:"版本 : 1.1.1"
   * 不管数据的多少大小,都是输出为一个数组
   */
  var versionNumberList = JSUtils.selNByJsoupXpath(
    userAgent,
    URL,
    "//div[@class='intro app-other-info-intro']/p[4]/text()"
  );
  /*
   * 获取版本号
   * 得到历史版本
   * 数据格式: [版本 : 1.1.1,版本 : 2.2.2]
   * 使用提供的addAll,可以将两个数组合并
   *
   */
  versionNumberList.addAll(
    JSUtils.selNByJsoupXpath(
      userAgent,
      URL,
      "//span[@class='history-verison-app-versionName']/text()"
    )
  );
  //Log.d(versionNumberList);

  /*
   * 获取下载链接
   * 获取对应"第一个版本"的链接
   * 链接比较特殊,需要使用正则去除数据中的链接
   * 取出输出的数组中的数据使用get函数
   * 此输出的数据:
   *
   *
   */
  var first_raw_url = JSUtils.selNByJsoupXpath(
    userAgent,
    URL,
    "//a[@class='download_app']/@onclick"
  ).get(0);
  //定义正则表达式
  var reg = /((ht|f)tps?:)\/\/[-A-Za-z0-9+&@#/%?=~_|!:,.;]+[-A-Za-z0-9+&@#/%=~_|]/g;
  //使用JavaScript的match函数配合正则表达式取出字符串中的下载链接
  var first_url = first_raw_url.match(reg)[0];

  //得到历史版本的下载链接,输出一个数组
  var releaseDownloadUrlList = JSUtils.selNByJsoupXpath(
    userAgent,
    URL,
    "//a[@class='historyVerison-download fright download_app']/@href"
  );
  /*
   * 将第一个链接和历史版本的链接合并
   * 使用add函数
   * add(位置,数据)
   * add(0,url)
   * 将第一次获得的链接使用add函数添加到数组的"0"起始位置
   */
  releaseDownloadUrlList.add(0, first_url);
  //Log.e(releaseDownloadUrlList);
  //获取软件名称,将它赋值
  var app_name=getDefaultName();
  
  
  //获取更新日志
  //第一个
  var changelog1 = JSUtils.selNByJsoupXpath(
    userAgent,
    URL,
    "//p[@class='art-content'][2]/text()"
  ).get(0);
  var changelog2=
    JSUtils.selNByJsoupXpath(
      userAgent,
      URL,
      "//div[@class='history-version-app-updateMsg']/text()"
    
  );
  //更新日志
  changelog2.add(0,changelog1);
  //Log.d(changelog2.size());
  //Log.d(changelog2.get(0));
  //Log.d(changelog2.get(1));
  //Log.d(changelog2.get(10));
  
  //将所有数据转化成json,并返回
  return  jsonstring(app_name,versionNumberList,releaseDownloadUrlList,changelog2);
}

function jsonstring(App_name,version_array,url_array,change){
  var datas = [];
  for (var i = 0; i < version_array.size(); i++) {
    var data = {};
    var assets = [];
    var asset = {};
    asset["name"] = "[" + App_name + "]" + version_array.get(i);
    asset["download_url"] = "" + url_array.get(i);
    assets.push(asset);
    data["version_number"] = "" + version_array.get(i);
    data["change_log"] ="" + change.get(i);
    data["assets"] = assets;
    datas.push(data);
  }
  return JSON.stringify(datas);
}

function getDefaultName() {
  var nodeList = JSUtils.selNByJsoupXpath(
    userAgent,
    URL,
    "//body/div/div[3]/div[1]/div[1]/div/h1/text()"
  );
  var defaultName = nodeList.get(0);
  Log.d(defaultName);
  return defaultName;
}

```

# 基本解析

## 结构要求

```text

JS 脚本结构必须包括：

    getDefaultName
    getReleaseInfo

```

## 调用的接口函数

### getDefaultName

获取默认名称。

> 输入：无  
> 输出：Java可变数组
> 说明：假如用户添加跟踪项时未设置名称，将会使用该项以获取默认名称。

### getReleaseInfo

从**UpgradeAll 1.1.1**开始，提供此函数。

此函数供开发者提供Json数据,返回给应用。

> 要求输出：Json数据

> 说明：Json数据中需要包含:
**应用名称、应用版本、下载链接**

> [ getReleaseInfo 详细说明  ](https://github.com/DUpdateSystem/UpgradeAll-rules/wiki/getReleaseInfo-%E8%AF%A6%E7%BB%86%E8%AF%B4%E6%98%8E)


## 预置函数

UpgradeAll 目前提供了一些 API，以函数的形式提供。

### 打印日志（Log）

- Log.v(String msg)
- Log.d(String msg)
- Log.i(String msg)
- Log.w(String msg)
- Log.e(String msg)


> 输入：字符串  
> 输出：无

```text

优先级是以下值之一：

    V — 详细（最低优先级）
    D — 调试
    I — 信息
    W — 警告
    E — 错误

```

### 爬虫工具（JSUtils）

#### Json

因为 Rhino 对于 Json 支持有限，UpgradeAll 提供了 Json 接口。  
JSUtils.getJSONObject()

> 输入：无  
> 输出：JSONObject

#### JSUtils.getJSONObject(String jsonString)

> 输出：JSONObject 字符串  
> 输出：JSONObject

#### JSUtils.getJSONArray()

> 输入：无  
> 输出：JSONArray

#### JSUtils.getHttpResponse(String URL)

OkHttp 支持（获取访问网址返回的数据，例如 Github API）

> 输入：网址字符串（需带有 HTTP 等响应协议，例如：<https://github.com>）  
> 输出：网站返回的字符串数据。

#### JSUtils.selNByJsoupXpath(String userAgent, String URL, String xpath)

Jsoup 支持（模拟浏览器访问网址，并使用 Xpath 获取网页中的数据）

> 输入：userAgent（用户代理，用于伪装浏览器）URL（访问的网址）xpath（Xpath 规则）  
> 输出：字符串（访问网页后，通过 Xpath 获取到的网页的数据）

# Rhino 特性解释及编程细节

> Rhino 是 Mozilla 开发的 JS 解释器，与众多 node.js 衍生库不同，它采用 Java 语言，而非 C++。大幅减少体积，并采用即时编译技术，加快重复使用的 JS 脚本运行速度。  
> 目前（2019-07-29）它尚不完善，不支持某些 JS 语法，例如：Class 关键词，Json 支持。  
> 但它可以直接调用 JAVA 库，并其语法与 Java 无异。  
> 例如创建 JSONObject 之后可以使用 put，get 函数，也可以捕捉产生的错误。

特性详见：[Rhino 开发者文档](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino/Scripting_Java)
