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
