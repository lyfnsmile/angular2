REGEX对象

这是对慕课网[javascript正则表达式](http://www.imooc.com/learn/706)的总结。

验证网站[https://regexper.com/](https://regexper.com/)

## 第一节 简介

javascript通过内置对象RegExp支持正则表达式，有两种方式实例化RegExp对象
- 字面量
- 构造函数

### 字面量
var reg=/\bis\b/g;//匹配单词is  \b单词边界符


### 构造函数
var reg=new RegExp('\\bis\\b')


- g global全文搜索
- i 忽略大小写
- m 多行搜素


## 第二节 元字符

正则表达式由两种基本字符串组成

- 原义文本字符
- 元字符


原义文本字符：字符串原本表示的含义
元字符： 如下

|  字符  |   含义   |
| :----: | :-----: |
|   \t   |水平制表符|
|   \v   |垂直制表符|
|   \n   |换行符|
|   \r   |回车符|
|   \o   |空字符|
|   \f   |换页符|
|   \cX  |与X对应的控制字符(Ctrl+X)|
|   \cZ  |与X对应的控制字符(Ctrl+Z)|


## 第三节 字符类

一般情况下正则表达式一个字符对应字符串的一个字符

### 字符类
- 我们可以使用元字符[]来构建一个简单的类
- 所谓类是指符合某些特性的对象，一个泛指。而不是特指某个字符

Example

```javascript
var reg=/[abc]/g;
'a1b2c3d4'.replace(reg,'x');

//x1x2x3d4

```


### 字符类取反
- 使用元字符^创建反向类/负向类
- 反向类的意思是不属于类的内容
例如表达式/^[abc]/表示不是字符a或b或c的内容

Example

```javascript
var reg=/^[abc]/g;
'a1b2c3d4'.replace(reg,'x');

//axbxcxxx

```


## 第四节 范围类

- 正则表达式还提供了范围类，由于表示某一范围的字符类。符号 '-'

- 所以我们可以使用/[a-z]/来表示从a到z的任意小写字符

Example

```javascript
var reg=/^[a-z]/g;
'a1b2c3d4'.replace(reg,'x');

//x1x2x3x4

```


## 第五节 预定义类及边界

### 预定义类

- 正则表达式提供了预定义类来匹配常见的字符类

|  字符  |   等价类   |   含义   |
| :----: | :-------: |  :----:  |
|   .   |回车和换行符以外的所有字符|
|   \d   |  [0-9]   |  所有数字  |
|   \D   |  ^[0-9]  | 所有非数字 |
|   \s   |        |空白符|
|   \S   |        |非空白符|
|   \w   | [a-zA-Z_0-9]   |单词字符(字母、数字、下划线)|
|   \W   | ^[a-zA-Z_0-9] |非单词字符   |

>  结合英文原义 d==>digit 数字    s==>space  空白    w==>word  单词


### 边界

- 正则表达式还提供了几个常用的边界匹配字符

|  字符  |  含义   |
| :-------: | :--------: |
|   ^    |以xxx开始|
|   $    |以xxx结尾|
|   \b   |单词边界|
|   \B   |非单词边界|

> 在非字符类中，^表示必须以XXX开始

Example

```javascript
var reg=/@\d/g;
'@23@31r@'.replace(reg,'x');

//"x3x1r@"

```

```javascript
var reg=/^@\d/g;
'@23@31r@'.replace(reg,'x');

//"x3@31r@"

```

## 第六节 量词

- 当我们希望匹配一个连续出现10次数字的时候、此时就是量词出场了。量词意即表示数量的词

如下:  /\d{10}/


|  字符  |  含义   |
| :-------: | :--------: |
|   ?    |出现0次或者1次|
|   +    |至少出现1次|
|   *    |出现0次或者任意次|
|   {n}  |出现n次|
| {n,m}  |出现n~m次|
|   {n,}  |至少出现n次|



## 第七节  贪婪模式与非贪婪模式 

- 贪婪模式 尽可能多的匹配。如下：

Example

```javascript
var reg=/\d{3,6}/g;
'12343234'.replace(reg,'x');

//"x34"

```

- 非贪婪模式  尽可能少的匹配

如果想要使用非贪婪模式，只需要在量词后面加上?即可

Example

```javascript
var reg=/\d{3,6}?/g;
'123456789'.replace(reg,'x');

//"xxx"

```

## 第八节  分组()和或|

### 分组

- 使用()可以达到分组的作用，是量词便于分组。


例如想要让单词'lizx'重复三次，可能你会写出/lizx{3}/,但是实际匹配的是lizxxx。

怎样才能达到我们的目的呢，就是分组。如下:

> 分组使用()表示

Example

```javascript
var reg=/(lizx){3}/g;
'lizxlizxlizxlizx23'.replace(reg,'李志祥');

//"李志祥lizx23"

```


### 或

- 使用|达到或的效果

Example

```javascript
var reg=/((li|l)zx)/g;
'lizxlzx23'.replace(reg,'李志祥');

//"李志祥李志祥23"

```

### 反向引用

- 如果想要实现这样的效果 将2016-10-20  ==>> 10/20/2016

正确的正则表达式是

```javascript
var reg=/(\d{4})-(\d{2})-(\d{2})/g;
'2016-10-20'.replace(reg,'$2/$3/$1');

//"10/20/2016

```

>  必须在分组中使用

### 忽略分组
- 如果不希望捕获某些分组，只需要在分组内加上?:就可以了

```javascript
var reg=/(?:\d{4})-(\d{2})-(\d{2})/g;
'2016-10-20'.replace(reg,'$2/$1');

//"20/10"

```

## 第九节  前瞻

- 正则表达式从文本头部向尾部解析，文本尾部方向称为"前"

- 前瞻就是在正则表达式匹配到规则的时候，向前检查是否符合断言

正则表达式还有一个与之对应的概念叫"后顾"，后顾就是向后检查是否符合断言

但是在javascript中并不支持这种后顾表达式


[符合和不符合某种断言有被称为肯定/正向匹配和否定/负向匹配]

|  名称      |正则表达式  |   含义    |
| :-------: | :--------: |:--------: |
|   正向前瞻    |exp(?=assert)|      |
|   负向前瞻    |exp(?!assert)|      |

(assert)：断言语句  exp:正则表达式


```javascript
var reg=/\w(?=\d)/g;
'a2*34v8'.replace(reg,'H');

//"H2*H4H8"

```


```javascript
var reg=/\w(?!\d)/g;
'a2*34v8'.replace(reg,'H');

//aH*3HvH

```


## 第十节  正则表达式对象属性


- 正则表达式属于对象，是对象就会有自身的属性。
例如：
var reg=/\w/gim;

其中： g、i、m都是正则表达式的属性。是不可修改的
```
reg.global//true

reg.ignoreCase//true
```


reg.source//正则表达式的文本内容  `/\w/gim`
reg.lastIndex  //当前匹配结果的最后一个字符的下一个字符的位置

## 第十一节  正则表达式对象方法


- RegEx.prototype.text(str):

由于测试特定字符串师傅符合正则表达式的规则，return value 类型为boolean

- RegEx.prototype.exec(str):

使用正则表达式模式对字符串执行搜索，并将更新全局RegExp对象的属性以反映匹配结果

如果没有匹配的文本则返回null，否则返回一个结果数组


## 第十二节  字符串相关方法

- String.prototype.search(reg)

可以接受字符或正则表达式

return Number。如果匹配到了返回其所在字符串中的位置，泛指返回-1

- String.prototype.match(reg)

return Array  如果没有匹配的文本则返回null

- String.prototype.split(reg)

可以接受字符或正则表达式

```
var reg=/\d/;

'0a1b2c3d4'.split(reg,[string|function])

//["", "a", "b", "c", "d", ""]

```

- String.prototype.replace(reg)

在前面的示例中多次使用到了replace方法

在这里演示第二个参数是Function类型的情况:

function在每次匹配替换的时候调用，最多接受四种参数

1. 匹配字符串
2. 正则表达式分组内容，没有分组则没有该参数
3. 匹配项在字符串中的位置
4. 原字符串



如果想要实现将匹配的数字都加一，应该怎么写呢???

```

var reg=/\d/g;

'w1y2ydg3d6'.replace(reg,function(match,index,origin){
    console.log(index)
    return parseInt(match)+1;
})


//1,3,7,9
//"w2y3ydg4d7"

```

再举一个包含分组的复杂例子;

过滤掉分组三的结果集

```
var reg=/(\d)(\w)(\d)/g;

'w1y2ydg3d6'.replace(reg,function(match,group1,group2,group3,index,origin){
    console.log(match,group1)
    return group1+group2
})


//1y2   // 3d6
//"w1yydg3d"
```


