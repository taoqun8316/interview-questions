
<details>
 <summary>PHP中几个输出函数echo，print()，print_r()，sprintf()，var_dump()的区别</summary>


1：echo：是语句不是函数，没有返回值，可输出多个变量值，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
2：print：是语句不是函数，有返回值 1 ，只能输出一个变量，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
3：print_r：是函数，可以打印复合类型，例如：stirng、int、float、array、object等，输出array时会用结构表示，而且可以通过print_r($str,true)来使print_r不输出而返回print_r处理后的值
4：printf：是函数，把文字格式化以后输出（参看C语言）
5：sprintf：是函数，跟printf相似，但不打印，而是返回格式化后的文字（该函数把格式化的字符串写写入一个变量中，而不是输出来），其    他的与printf一样。
6：var_dump()：函数，输出变量的内容、类型或字符串的内容、类型、长度。常用来调试。

</details>




<details>
 <summary>不用新变量直接交换现有两个变量的值</summary>

1.用list()
```
   a=10;b = 25;
   list(a,b)=array(b,a);

   echo a.′/′.b;//25/10
```
2.用传值方式加&符
```
  function swap(&a,&b){

      $c = '';

      c=a;

      a=b;

      b=c;

}

      a=25;b = 35;
      swap(a,b);

echo a.′/′.b;
```

方法三：使用数组分割
```
    a="123";b = "456";

    b=a."#".b;

    b = explode("#",b);a = b[1];b = b[0];echoa."-".$b;
```
 

方法四：使用异或运算
```
    a="fsdfds";b = "xiaorui";

    a=a^b;b = b^a;

    a=a^b;echo a."-".$b;
```
</details>



<details>
 <summary><h4>heredoc</h4></summary>

>Heredoc在正规的PHP文档中和技术书籍中一般没有详细讲述。他是一种Perl风格的字符串输出技术。使用heredoc技术可以实现界面与代码的准分离，比如phpwind模板。

heredoc的语法是用”<<<”加上自己定义成对的标签，在标签范围內的文字视为一个字符串

规则如下：

1、以<<<End开始标记开始，以End结束标记结束，结束标记必须顶头写，不能有缩进和空格，且在结束标记末尾要有分号 。开始标记和开始标记相同，比如常用大写的EOT、EOD、EOF来表示，也可以使用其他标记，只要保证开始标记和结束标记不在正文中出现就行。

2、位于开始标记和结束标记之间的变量可以被正常解析，但是函数则不可以。在heredoc中，变量不需要用连接符 . 或 , 来拼接，比如：
```
$a=2;
$b= <<<EOF
  "zyf"$a
  "zyf"
EOF;
echo $b; //结果连同双引号一起输出："zyf"2 "zyf"
```
3、heredoc常用在输出包含大量HTML语法文档的时候。他要比传统的echo输出精炼很多，如下所示：

```
function getHtml()
{
    echo "<html>";
    echo "<head><title>Title</title></head>";
    echo "<body>Content</body>";
    echo "</html>;
}

function getHtml()
{
echo <<<EOT
      <html>
      <head><title>Title</title></head>
      <body>Content</body>
      </html>
EOT;
}
```
</details>



<details>
 <summary>heredoc</summary>
1
</details>
