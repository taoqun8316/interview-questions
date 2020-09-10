
<details>
 <summary>PHP中几个输出函数echo，print()，print_r()，sprintf()，var_dump()的区别</summary>

```
1：echo：是语句不是函数，没有返回值，可输出多个变量值，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
2：print：是语句不是函数，有返回值 1 ，只能输出一个变量，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
3：print_r：是函数，可以打印复合类型，例如：stirng、int、float、array、object等，输出array时会用结构表示，而且可以通过print_r($str,true)来使print_r不输出而返回print_r处理后的值
4：printf：是函数，把文字格式化以后输出（参看C语言）
5：sprintf：是函数，跟printf相似，但不打印，而是返回格式化后的文字（该函数把格式化的字符串写写入一个变量中，而不是输出来），其    他的与printf一样。
6：var_dump()：函数，输出变量的内容、类型或字符串的内容、类型、长度。常用来调试。
```
</details>


<details>
 <summary>不用新变量直接交换现有两个变量的值</summary>

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
</details>