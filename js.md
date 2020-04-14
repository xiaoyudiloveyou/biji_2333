### js

#### 目录:
>关键字：
  1. [let和const](https://www.jianshu.com/p/7bba55633028)
>构建
  1. [module.exports用法](https://blog.csdn.net/xqnode/article/details/60610885)
  2. [moudel.exports 导出模块](https://www.cnblogs.com/luxiaoyao/p/8640063.html)
  3. [JS中的「import」和「require 」](https://www.jianshu.com/p/f1e54dde30c8)
  4. [使用npm init初始化项目](https://www.cnblogs.com/WD-NewDemo/p/11141384.html)
>基础应用
     
#### 基本数据类型操作
##### 1. Nan校验:
   *  number数字类型:包括数字和NaN，NaN：not a number  
   [链接](https://www.cnblogs.com/jiajialove/p/10998710.html)
   
##### 2. js 字符串转换数字: js提供了parseInt()和parseFloat()两个转换函数。前者把值转换成整数，后者把值转换成浮点数。只有对String类型调用这些方法，这两个函数才能正确运行；对其他类型返回的都是NaN(Not a Number)。  
    [连接](https://www.cnblogs.com/carekee/articles/1729574.html)  
   * 其他：var b = Number('11')
   
##### date类型操作:    
1.code:
```javascript 1.8
js时间字符串转Date对象
   原创baibaibai_1003 最后发布于2018-10-09 13:11:22 阅读数 11708  收藏
   展开
   var s ='2018-10-09 10:23:12';
   s = s.replace(/-/g,"/");
   var date = new Date(s );
```

2.code:
```javascript 1.8
function timestampSub(timestart,timeend){
    // 两个时间格式的时间转时间戳之后再相减
     var timestamp = Date.parse(timeend) - Date.parse(timestart);
    // 取绝对值
    timestamp = Math.abs(timestamp);
    // 除以一天的毫秒数（默认时间戳是到毫秒的，就算取到秒级的时间戳后面也带了3个0）
    timestamp = timestamp / (24 * 3600 * 1000);
    // 取整
    timestamp = Math.floor(timestamp);
    return timestamp;
}
//-----------------------------------
```

3.js转换时间戳-转换成 yyyy-MM-dd HH:mm:ss  
比如：转换成 yyyy-MM-dd HH:mm:ss
```javascript 1.8
//时间戳转换方法    date:时间戳数字
function formatDate(date) {
    var date = new Date(date);
    var YY = date.getFullYear() + '-';
    var MM = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
    var DD = (date.getDate() < 10 ? '0' + (date.getDate()) : date.getDate());
    var hh = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours()) + ':';
    var mm = (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) + ':';
    var ss = (date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds());
    return YY + MM + DD +" "+hh + mm + ss;
}

```

4. code:
```javascript 1.8
javascript 时间日期处理相加，减操作方法js
(2011-04-22 18:08:51)
转载▼
标签：
杂谈
 
[JavaScript]2009-02-21 12:01:32 阅读523 评论0字号：大中小 订阅

<script language="JavaScript">
<!--
var d = new Date("2008/04/15");
d.setMonth(d.getMonth() + 1 + 1);//加一个月，同理，可以加一天：getDate()+1，加一年：getYear()+1
alert(d+"月后是"+d.getFullYear()+"-"+d.getMonth()+"-"+d.getDate());
//时分秒：
var d2 = new Date("2008/04/15 02:03:04");
alert(d2.getHours()+"-"+d2.getMinutes()+"-"+d2.getSeconds());
//-->
</script>
 
可以用date对象来实现~~
比如:var date = new Date()(2011,03,09);搜索
那加上10个月连20天:
date.setMonth(date.getMonth()+10);
date.setDate()(date.getDate()()+20);
alert(date);//2012年2月29日
~~~
```

5.javascript 时间日期处理相加，减操作方法js
```javascript 1.8
function dateAddDays(dataStr,dayCount){
    var strdate = dataStr; // 2017年03月01日，该日期增加dayCount天
    strdate=strdate.replace("年","/");
    strdate=strdate.replace("月","/");
    strdate=strdate.replace("日","/");
    var isdate = new Date(strdate);
    isdate = new Date((isdate/1000+(86400*dayCount))*1000); // dayCount=1
    var year = isdate.getFullYear(); // yyyy
    var month = isdate.getMonth()+1; // M
    var day = isdate.getDate(); // d

    if (month >= 1 && month <= 9) { // MM
        month = "0" + month;
    }
    if (day >= 0 && day <= 9) { // dd
        day = "0" + day;
    }

    var pdate = year+"年"+month+"月"+day+"日"; // pdate=2017年03月02日
    return pdate;
}
```
可以用date对象来实现
比如:var date = new Date()(2011,03,09);搜索
那加上10个月连20天:
date.setMonth(date.getMonth()+10);
date.setDate()(date.getDate()()+20);

6.js实现时间日期的相加
```javascript 1.8
<script>
function DateAdd(interval,number,date)
{
/*
 *--------------- DateAdd(interval,number,date) -----------------
 * DateAdd(interval,number,date) 
 * 功能:实现VBScript的DateAdd功能.
 * 参数:interval,字符串表达式，表示要添加的时间间隔.
 * 参数:number,数值表达式，表示要添加的时间间隔的个数.
 * 参数:date,时间对象.
 * 返回:新的时间对象.
 * var now = new Date();
 * var newDate = DateAdd("d",5,now);
 * author:wanghr100(灰豆宝宝.net)
 * update:2004-5-28 11:46
 *--------------- DateAdd(interval,number,date) -----------------
 */
    switch(interval)
    {
        case "y" : {
            date.setFullYear(date.getFullYear()+number);
            return date;
            break;
        }
        case "q" : {
            date.setMonth(date.getMonth()+number*3);
            return date;
            break;
        }
        case "m" : {
            date.setMonth(date.getMonth()+number);
            return date;
            break;
        }
        case "w" : {
            date.setDate(date.getDate()+number*7);
            return date;
            break;
        }
        case "d" : {
            date.setDate(date.getDate()+number);
            return date;
            break;
        }
        case "h" : {
            date.setHours(date.getHours()+number);
            return date;
            break;
        }
        case "m" : {
            date.setMinutes(date.getMinutes()+number);
            return date;
            break;
        }
        case "s" : {
            date.setSeconds(date.getSeconds()+number);
            return date;
            break;
        }
        default : {
            date.setDate(d.getDate()+number);
            return date;
            break;
        }
    }
}

var now = new Date();
//加五天.
var newDate = DateAdd("d",5,now);
alert(newDate.toLocaleDateString())
//加两个月.
newDate = DateAdd("m",2,now);
alert(newDate.toLocaleDateString())
//加一年
newDate = DateAdd("y",1,now);
alert(newDate.toLocaleDateString())
</script>
```

7.js一行代码计算两个时间相差的天数
```javascript 1.8
iDays = Math.floor(Math.abs(Date.parse(new Date()) - Date.parse(row.create_time)) / (24 * 3600 * 1000));
console.log(iDays);
```
拆开来就是：
```
// 两个时间格式的时间转时间戳之后再相减
timestamp = Date.parse(new Date()) - Date.parse(row.create_time)
// 取绝对值
timestamp = Math.abs(timestamp)
// 除以一天的毫秒数（默认时间戳是到毫秒的，就算取到秒级的时间戳后面也带了3个0）
timestamp = timestamp / (24 * 3600 * 1000);
// 取整
timestamp = Math.floor(timestamp);
```
    参考的网站：  
https://www.cnblogs.com/jingwhale/p/4674946.html
https://www.cnblogs.com/gaocong/p/6781573.html
https://blog.csdn.net/u012302552/article/details/83305450

##### JavaScript函数中的参数类型  
[连接](https://blog.csdn.net/u012868077/article/details/51588145)

##### JS语法之：map()方法  
[JS中的Map对象](https://www.cnblogs.com/mywangpingan/p/10309058.html)  
[其他链接](https://blog.csdn.net/liminwang0311/article/details/86480829)  

##### 正则
1.js正则表达式中/=\s*\".*?\"/gsd  
[链接](https://www.cnblogs.com/wanshutao/p/3518406.html)  
2.JS的正则表达式限定开始和结尾等测试  
[链接](https://www.cnblogs.com/huaxie/p/11833496.html)  
3.js -- 正则表达式集合    
[链接](https://www.cnblogs.com/lixingwu/p/7280964.html)  
4.js 匹配任意字符  
[链接](https://www.jianshu.com/p/2718c58c2e9b  
5.String对象的方法  
[链接](https://www.cnblogs.com/rxbook/p/11820720.html)   
6.js在字符串中加入一段字符串  
[链接](https://www.cnblogs.com/mei1234/p/10452546.html)   
```javascript 1.8
var str = "Hello,world!";
var newStr = str.slice(0,5)+'-local'+str.slice(5)  //Hello-local,world!
```
7.js 字符串插入删除操作
```javascript 1.8
1、插入值

var str = 'abde'

str = str.slice( 0 , 2 ) + 'c' + str.slice( 2 )  //'abcde'

2、删除任意位置的值

var str2 = 'abcde'

str2 = str2.split( str2.slice( 1 , 2 )).join( '' )  //'acde'（删除1个）

字符串方法 slice：

　　slice(beginInd,endInd) ==> 截取从beginInd开始到endInd之间的字符串，不改变原字符串

字符串方法 split：

　　split(Str|Reg,limited) ==> 将字符串中符合Str或正则Reg的字符串去掉，返回一个数组，不改变原来字符串
```

8.

##### 定义list  
  1. [js声明一个数组，声明一个map](https://blog.csdn.net/shuair/article/details/79945071)   
  2. [javascript定义一个list](https://blog.csdn.net/weixin_30617695/article/details/102282134?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1)  
  3. [JavaScript可以定义一个List](https://blog.csdn.net/happydecai/article/details/100072598)  
##### 数组去重  
  1. [https://www.cnblogs.com/Koaler/p/12060917.html](https://www.cnblogs.com/Koaler/p/12060917.html)  
  2. 如下
```javascript 1.8
//对"\"去重
function uniq(array) {
    var temp = [];
    var index = [];
    var l = array.length;
    for (var i = 0; i < l; i++) {
        for (var j = i + 1; j < l; j++, i++) {
            if (array[i] != array[j]) {
                break;
            }
        }
        temp.push(array[i]);
        index.push(i)
    }

    return temp;
}

var aa = ["root\\\\\\u534e\\u4e3a\\u4ea7\\u54c1\\\\\\u5b58\\u50a8"];
// console.log(uniq(aa))
// console.log(aa)
```

##### unicode转换
eg.1
```javascript 1.8
// 转为unicode 编码  
function encodeUnicode(str) {  
    var res = [];  
    for ( var i=0; i<str.length; i++ ) {  
    res[i] = ( "00" + str.charCodeAt(i).toString(16) ).slice(-4);  
    }  
    return "\\u" + res.join("\\u");  
}  
  
// 解码  
function decodeUnicode(str) {  
    str = str.replace(/\\/g, "%");  
    return unescape(str);  
} 
```
eg.2
```javascript 1.8
// 中文转换为Unicode编码
var str = "我是张三";
escape(str).replace(/\%u/g,'/u');

// Unicode编码转换为中文
var str = "\u6211\u662F\u5F20\u4E09";
// 方法一：
// eval("'" + str + "'");
// 方法二：
// unescape(str);
```
eg.3: 其他
  1. [JS将unicode码转中文方法](https://blog.csdn.net/qq_43679771/article/details/87893338)  
  
##### json对象、数组和string互换  
  1. [javascript中json对象json数组json字符串互转及取值](https://www.cnblogs.com/tangbang/p/tb.html)  
  2. [浅谈JSON.parse()、JSON.stringify()和eval()的作用](https://www.cnblogs.com/DTBelieve/p/5346603.html)        
