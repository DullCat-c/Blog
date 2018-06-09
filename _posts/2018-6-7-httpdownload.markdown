---
layout:     post
title:      "PHP文件下载函数"
subtitle:   "可用于多人大文件下载"
date:       2018-6-7
author:     "DullCat"
header-img: "https://upload-images.jianshu.io/upload_images/10052831-74fa0d8425062e3a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"
tags:
    PHP
---

安利两种php的下载函数

1.`readfile`,获取文件的句柄(**注意:这里是句柄而不是文件,所以只占用很小的内存**)并将句柄输出到缓冲区
```
function readfile_download($url, $filename,$opt = null)
{
    //设置http下载消息报文
    header("Content-Disposition:  attachment;  filename=" . $filename);
    header("Pragma:  no-cache");
    header("Expires:  0");
    //有些下载需要附带cookie和useragent
    $cookie = $opt['cookie'] ? $opt['cookie'] : '';
    $useragent = $opt['useragent'] ? $opt['useragent'] : '';
    $opts = array(
        'http' => array(
            'method' => 'GET',
            'header' =>
                "UserAgent:$useragent\r\n" .
                "Cookie:$cookie \r\n",
        )
    );
    $context = stream_context_create($opts);
    readfile($url,false,$context);
}
```

---

但是缓冲区同样有限制大小,默认的缓冲区只有4k,一旦缓冲区溢出,同样也会占用内存,所以如果要进行多人大文件下载,缓冲区也要被限制

---
2.使用`fopen`获取远程文件的句柄,然后使用`fread`分段获取并输出,这样不仅占用的内存少,而且占据的缓冲区也少,可以用于多人大文件下载场景

```
function0 fopen_download($url, $filename,$opt = null,$limit = 1024)
{
    //设置http下载消息报文
    header("Content-Disposition:  attachment;  filename=" . $filename);
    header("Pragma:  no-cache");
    header("Expires:  0");
    //有些下载需要附带cookie和useragent
    $cookie = $opt['cookie'] ? $opt['cookie'] : '';
    $useragent = $opt['useragent'] ? $opt['useragent'] : '';
    $opts = array(
        'http' => array(
            'method' => 'GET',
            'header' =>
                "UserAgent:$useragent\r\n" .
                "Cookie:$cookie \r\n",
        )
    );
    $context = stream_context_create($opts);
    $handle = fopen($url, "r", false, $context);
    //输出
    while (!feof($handle)) {
        $content = fread($handle, intval($limit));
        echo $content;
        ob_flush();
    }
}
```

3.有些考虑到安全的项目,是会禁用fopen打开URL,又或是考虑到打开URL的稳定性和性能,所以想使用`cURL`函数

```
function download($url,$filename,$opt=null,$limit = 1024)
{
    //curl获取远程文件并储存在临时文件内
    $ch = curl_init();
    $fp = tmpfile();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_FILE, $fp);
    $cookie = $opt['cookie'] ? $opt['cookie'] : null;
    $useragent = $opt['useragent'] ? $opt['useragent'] : $_SERVER['HTTP_USER_AGENT'];
    curl_setopt($ch, CURLOPT_COOKIE, $cookie);
    curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
    curl_exec($ch);
    curl_close ($ch);
    //设置http下载消息报文
    header("Content-Disposition:  attachment;  filename=" . $filename);
    header("Pragma:  no-cache");
    header("Expires:  0");
    rewind($fp);
    //输出
    while (!feof($fp) && is_resource($fp)) {
        $content = fread($fp, $limit);
        echo $content;
        ob_flush();
    }
    //关闭并销毁临时文件
    fclose($fp);
}
```