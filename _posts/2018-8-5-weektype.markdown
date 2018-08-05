---
layout:     post
title:      "PHP的弱类型安全隐患及防范"
subtitle:   "PHP的弱类型安全隐患,写码尽量使用全等符号"
date:       2018-8-5
author:     "DullCat"
header-img: "https://upload-images.jianshu.io/upload_images/10052831-90599133833f75bf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"
tags:
    PHP
    Security
---

###### 字符串与数字对比
原理:
```
$a = '1DullCat';
$b = 'DullCat1';
var_dump($a == 1); //true
var_dump($b == 1); //false
intval($a); //1
intval($b); //0
```
php的对比运算时,是会将两个变量都转换为相同类型,不同变量的无法对比
当字符串和int类型对比时,将会把字符串转化为int类型,相当于字符串在底层执行了一次intval()函数.
至于为什么数字放前面就能intval,放后面就不行,是因为:
`该字符串的开始部分决定了它的值，如果该字符串以合法的数值开始，则使用该数值，否则其值为0。`
怪不得有些网站注册时非要名字以字符串开头....

注意:如果字符串在比较时带有'e','E'时,会被解析为科学计数法,如`var_dump('1e123' == '1');`为false

案例1:
```
$type = $_POST['type']
 if($type == 1){
  $sql = "select * from test where type = $type";
}
mysql_query($sql);
```
案例2:
```
$type = $_POST['type']
switch($type)
{
    case 1:
            $sql = "select * from test where type = $type";
             break;
    default:
            die();
}
mysql_query($sql);
```

延伸:
如果字符串和数字比较会出现类型转换的问题,那么一些php自带函数会不会出现相同的问题?

`in_array()`
```
$haystack = array(1,2,3,4);
$needle= '1DullCat';
var_dump(in_array($needle,$haystack)); //true
var_dump(in_array($needle,$haystack,true)); //false
```

虽然也会出现问题,但是一直都有解决的方案,in_array()存在第三个参数,决定是否严格检查,
默认为false , 相当与 "==" , 传入`true` ,相当于"==="
还有很多这样的函数,如`array_search()`,这里不一一介绍了

###### 函数返回值与数字对比

`strcmp()`
用来比较字符串和字符串,如果相等则返回0
一般有人会这么写
```
$a = $_POST['str'];
if(strcmp($a,'DullCat') == 0){
  //执行
}
```
这段代码便可以使用传入数组来绕过
因为用strcmp()对比数组和字符串时返回值为`null`, 但是 `null == 0`为true ,所以继续执行

`md5()`
函数传入数组会返回null;
但是本猫看了下网上的例子, 真的认为完全不合逻辑,就暂时一笔带过


>总结
>一定要做好参数的过滤,在使用他之前
>应该常用 "===" 来判断,而不是"=="
>应该了解每一个常用函数的参数和返回值,尽量采用严格模式