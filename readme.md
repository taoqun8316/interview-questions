### 网络

##### HTTP Keep-Alive的作用
```
作用：
Keep-Alive：使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接。Web服务器，基本上都支持HTTP Keep-Alive。

缺点：
对于提供静态内容的网站来说，这个功能通常很有用。但是，对于负担较重的网站来说，虽然为客户保留打开的连 接有一定的好处，但它同样影响了性能，因为在处理暂停期间，本来可以释放的资源仍旧被占用。当Web服务器和应用服务器在同一台机器上运行时，Keep- Alive功能对资源利用的影响尤其突出。

解决：
Keep-Alive: timeout=5, max=100
timeout：过期时间5秒（对应httpd.conf里的参数是：KeepAliveTimeout）

max：最多100次请求，强制断掉连接。就是在timeout时间内又有新的连接过来，同时max会自动减1，直到为0，强制断掉。
```

*** 
### PHP

##### PHP中几个输出函数echo，print()，print_r()，sprintf()，var_dump()的区别
```
1：echo：是语句不是函数，没有返回值，可输出多个变量值，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
2：print：是语句不是函数，有返回值 1 ，只能输出一个变量，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
3：print_r：是函数，可以打印复合类型，例如：stirng、int、float、array、object等，输出array时会用结构表示，而且可以通过print_r($str,true)来使print_r不输出而返回print_r处理后的值
4：printf：是函数，把文字格式化以后输出（参看C语言）
5：sprintf：是函数，跟printf相似，但不打印，而是返回格式化后的文字（该函数把格式化的字符串写写入一个变量中，而不是输出来），其    他的与printf一样。
6：var_dump()：函数，输出变量的内容、类型或字符串的内容、类型、长度。常用来调试。
```

##### 不用新变量直接交换现有两个变量的值
```
1.用list()

   a=10;b = 25;
   list(a,b)=array(b,a);

   echo a.′/′.b;//25/10

2.用传值方式加&符

  function swap(&a,&b){

      $c = '';

      c=a;

      a=b;

      b=c;

}

      a=25;b = 35;
      swap(a,b);

echo a.′/′.b;

方法三：使用数组分割

    a="123";b = "456";

    b=a."#".b;

    b = explode("#",b);a = b[1];b = b[0];echoa."-".$b;

 

方法四：使用异或运算

    a="fsdfds";b = "xiaorui";

    a=a^b;b = b^a;

    a=a^b;echo a."-".$b;
```


<details>
<summary>Title</summary>

content!!!
</details>