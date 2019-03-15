# html
1. lang: 语言类型
# 1. head
## 1.1 title
标题，元素 `<title>` 的内容也被用在搜索的结果中
## 1.2 meta
元数据
1. charset="utf-8"
2. name: meta元素的类型,description被使用在搜索引擎显示的结果页中
3. content: 实际的元数据内容
例如，添加作者和描述<br>
```
<meta name="author" content="HTML Article">
``` 

description
```
<meta name="description" content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both
Web sites and HTML5 Apps. It also documents Mozilla products, like Firefox OS.">
```

## 1.3 link
1. rel: 文件  类型
2. href: 文件的路径
## 1.4 script
1. src: 文件的路径
# 2. body
## 2.1 文本
### 2.1.1 标题和段落（文本的层次结构）
1. p, 
2. h1-h6
### 2.1.2 列表
1. ul
2. ol
3. li
### 2.1.3 重点强调
1. em: 强调
2. strong: 非常重要
### 2.1.4 文字意义
1. i
2. b
3. u
4. span
### 2.1.5 超链接(a)
属性：
1. href: 超链接地址
```
// 电子邮件链接
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a> 
```
2. title: 链接支持信息，（当链接悬停在其上时，标题将作为工具提示出现）
3. download: 下载的文件路径

### 2.1.6 描述列表
1. dl(description list) 
2. dt(description term)
3. dd(description description)
### 2.1.7 引用
1. blockquote: 块引用, 属性cite: 引用的资源
2. q: 行内引用, 属性cite: 引用的资源
### 2.1.8 缩略语
1. abbr 属性title 缩略语
### 2.1.9 联系方式
1. address
### 2.1.10 上下标
1. sup
2. sub
### 2.1.11 代码
1. code
2. pre
3. var
4. kbd
5. samp
### 2.1.12 日期
1. time， 属性datatime：机器识别格式，例如2019-03-07

## 2.2 组成区段
### 2.2.1 header
简介形式的内容
### 2.2.2 nav
导航
### 2.2.3 main
每个页面独有内容，单例
### 2.2.4 article
文章
### 2.2.5 section
按功能分块
### 2.2.6 aside
侧边栏,与主信息相关的间接信息
### 2.2.7 footer
页脚
### 2.2.8 其他
1. div
2. br
3. hr
### 2.2.9 html debug
1. [Markup Validation Service](https://validator.w3.org/)

## 2.3 多媒体与嵌入
### 2.3.1 img 图片
1. src: 图片地址
2. alt： 对图片的文字描述，用于图片无法显示或不能被看到的情况。
3. width/height
4. title

|key|value|
|:--|:--|
|srcset|资源地址 图像固有宽度，例：```elva-fairy-320w.jpg 320w```|
|sizes|媒体查询，与srcset一一对应, 例：```(max-width: 320px) 280px```|


### 2.3.2 figure+figccaption 图片+说明文字
在标题和图片之间建立清晰的关联
### 2.3.3 vedio + audio 视频和音频
1. video

|key|value|
|:--|:--|
|src|资源地址|
|control|控制，无value|
|width/height||
|autoplay|立即播放,无value|
|loop|循环播放,无value|
|muted|默认关闭声音,无value|
|poster|图片资源地址|
|preload|none/auto/metadata|

source: 多资源标签，l浏览器检测source标签的type属性,不支持的类型会被跳过。
|key|value|
|:--|:--|
|src|资源地址|
|type|类型，例如type="video/mp4"|

track标签 
|key|value|
|:--|:--|
|kind|样式 subtitles(字幕) / captions / timed descriptions|
2. audio
属性与video相似

### 2.3.4 SVG(矢量图)
```
<svg version="1.1"
     baseProfile="full"
     width="300" height="200"
     xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="black" />
  <circle cx="150" cy="100" r="90" fill="blue" />
</svg>
```

### 2.3.5 picture
```
<picture>
  <source media="(max-width: 799px)" srcset="elva-480w-close-portrait.jpg">
  <source media="(min-width: 800px)" srcset="elva-800w.jpg">
  <img src="elva-800w.jpg" alt="Chris standing up holding his daughter Elva">
</picture>
```
## 2.4 表格
### 2.4.1 基础
1. table
2. tr(table row)
3. td(table data)
4. th(table head)

|key|value|
|:--|:--|
|rowspan|占据的行数|
|colspan占据的行列数|占据的列数|

### 2.4.2 高级特性和可访问性
1. caption 标题
2. thead 第一行
3. tfoot 最后一行
4. tbody
<p>scope: th标签的属性，帮助屏幕阅读器</p>

## 2.5 表单
### 2.5.1 form
### 2.5.2 lable
### 2.5.3 input
type属性的value
1. text
2. checkbox
3. radio
4. email
5. password
6. search
7. tel
8. url
9. number min max step
10. range
11. datetime-local
12. month
13. time
14. week
15. date
16. color
17. file
18. hidden
19. image
### 2.5.4 select + option
下拉框
### 2.5.5 属性
1. autofocus
2. disbaled
3. name
4. value
5. multiple
### 验证
css
```
/* 验证失败的元素样式 */
input.invalid{
  border-color: #900;
  background-color: #FDD;
}

input:focus.invalid {
  outline: none;
}

/* 错误消息的样式 */
.error {
  width  : 100%;
  padding: 0;
 
  font-size: 80%;
  color: white;
  background-color: #900;
  border-radius: 0 0 5px 5px;
 
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

.error.active {
  padding: 0.3em;
}
```
1. required 
2. pattern 正则验证
3. min/max
### 2.5.6 textarea
1. cols
2. rows
### 2.5.7 datalist
```
  <label for="myFruit">What is your favorite fruit? (With fallback)</label>
  <input type="text" id="myFruit" name="fruit" list="fruitList">
  <datalist id="fruitList">
    <label for="suggestion">or pick a fruit</label>
    <select id="suggestion" name="altFruit">
      <option>Apple</option>
      <option>Banana</option>
      <option>Blackberry</option>
      <option>Blueberry</option>
      <option>Lemon</option>
      <option>Lychee</option>
      <option>Peach</option>
      <option>Pear</option>
    </select>
  </datalist>
```

### 2.5.8 button
type属性
1. submit
2. reset
3. button

### 2.5.9 其他
1. progress
2. meter
3. fieldset 表单分组 legend： 分组的标题
xss 将脚本注入WEB页面，从而绕过同源策略
csrf: 试图设计特权
SQL注入：发送一个SQL请求，希望服务器能够执行它
HTTP数据头注入和电子邮件注入