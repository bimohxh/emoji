# 关于 
![demo](https://github.com/awesomes-cn/emoji/blob/master/images/demo.png?raw=true)

该插件是对 emoji 表情包进行的一个封装，能够很好地运用到我们的项目中去，具有如下特点：

1. 可在页面任何地方加入

2. 可在单个页面加入多个实例而不会引起冲突

3. 可选择获取到插入的表情标识或表情图片路径

4. 可定制表情包的分类

5. 提供多种参数自定义

# 使用方法

1、引入 jquery 文件，这里咱们可以直接引用百度的cdn
``` xml
<script src="http://libs.baidu.com/jquery/1.9.0/jquery.min.js"></script>
```
2、引入underscore.js 文件，该文件是提供集合的处理
``` xml
<script type="text/javascript" src="js/underscore-min.js"></script>
```
3、选择性引入 editor.js 文件。该文件提供了一个文本框插入文本的方法，仅用于需要自己写编辑器插入表情的情况。
``` xml
<script type="text/javascript" src="js/editor.js"></script>
```
4、引入改插件的主js文件
``` xml
<script type="text/javascript" src="js/emojis.js"></script>
```
5、引入 css 文件，该文件定义了表情选择器的整个样式，当然也可以自己改写其中的某些样式。
``` xml
<link rel="stylesheet" type="text/css" href="css/emoji.css">
```
6、指定一个html标签容器供容纳表情
``` xml
<div id="emoji" class="emoji-container">
    <img  src="emoji/unicode/1f604.png" class="emoji-tbtn">
</div>
```
该容器包含了一个div和内部的一个img标签。注意：div必须指定ID，默认给了一个 `emoji-container` 的样式，该样式主要定义了一个 position:relative。保证整个布局不会乱。里面的 img 标签实际上是选择表情的图标，注意里面有个 `emoji-tbtn` 的class。

7、初始化插件
``` xml
$('#emoji1').emoji({
  //- 参数列表
});
```
参数详解

### 1、data

表情数据，是一个json数组格式如下：
``` javascript
data: [
  {
    "typ":"EmojiCategory-People",
    "nm":"人物",
    "items":[
      {"name":"smile","src":"unicode/1f604.png"},
      {"name":"smiley","src":"unicode/1f603.png"}
    ]
  },
  {
    "typ":"EmojiCategory-Nature",
    "nm":"自然",
    "items":[
      {"name":"smile","src":"unicode/1f604.png"},
      {"name":"smiley","src":"unicode/1f603.png"}
    ]
  },
  .....
]
```
默认已经有一个表情的数据了，所以一般情况下不需要管这个参数。

###  2、path

表情图片的路径

###  3、category

要显示的表情分类。默认的完整分类为：
``` javascript
category: ['EmojiCategory-People','EmojiCategory-Nature','EmojiCategory-Objects','EmojiCategory-Places','EmojiCategory-Symbols']
```
可以根据自己的需要来增加或删除分类。

### 4、showbar

是否显示左侧的分类菜单，如果是只显示一个分类，可以选择不显示

###  5、trigger

分类切换的事件，默认为` click`，即鼠标点左侧的击分类菜单切换标签包，可设置成任何鼠标事件，比如` mouseover `即鼠标划过的时候切换。

## 事件

###  1、insertAfter

这是该插件唯一的事件，表示点击每个表情图片发生的事件。例如我们要将表情插入到文本框里面去，我们可以这样写。
``` javascript
$(function(){
  var em = $('#emoji').emoji({
     insertAfter: function(item){
        $('#area').insertContent('[:'+item.name+':]')
      }
   });
})
```
其中 item 参数是一个json对象，包括一个` name `和一个` src`。

**name** 是该表情的字符串标识。

**src** 表示表情图片的路径。

 **数据处理**

该表情包是处理前端的展示，最终我们将数据存到了数据库里面，比如一条留言，那么我们该怎么样来展示这条留言呢？有两种方式来阻止文本：

**1、直接存表情图片路径**

如果我们将表情图片的路径存到了数据库里面，那么直接显示即可，但是这种方式会将图片路径写死，且不易改变数据，很不灵活，所以不推荐使用。

**2、存表情的标识**

即我们上面提到的 insertAfter 回掉里面的 item.name 。然后我们可以通过正则去将标识替换成图片。

标识的格式可以为

`:smile:`

`[smile]`

`[:smile:]`

也可以定义成我们自己的格式，只要保证插入表情标识的格式和解析的格式一致就OK了。

###  更新

原插件中的效果是，页面初始化后默认加载第一个分类的所有图片，如果你觉得这会带来额外的带宽，可以选择在点击插入表情图标后才初始化整个插件，而后才加载图片。那么你可以将 emojis.js 中的：
``` javascript
return this.each(function(){
      render_nav(this,options)
      render_emoji(this,options)
      switchitem(this,options)
      togglew(this,options)
    })
```
修改成
``` javascript
return this.each(function(){
  var obj = this
  $(this).find('.emoji-tbtn').one('click',function(){
        render_nav(obj,options)
        render_emoji(obj,options)
        switchitem(obj,options)
        togglew(obj,options)
   })
})
```
