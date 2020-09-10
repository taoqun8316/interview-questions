
<details>
 <summary><b>PHP中几个输出函数echo，print()，print_r()，sprintf()，var_dump()的区别</b></summary>


1. echo：是语句不是函数，没有返回值，可输出多个变量值，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
2. print：是语句不是函数，有返回值 1 ，只能输出一个变量，不需要圆括号。不能输出数组和对象，只能打印简单类型(如int,string)。
3. print_r：是函数，可以打印复合类型，例如：stirng、int、float、array、object等，输出array时会用结构表示，而且可以通过print_r($str,true)来使print_r不输出而返回print_r处理后的值
4. printf：是函数，把文字格式化以后输出（参看C语言）
5. sprintf：是函数，跟printf相似，但不打印，而是返回格式化后的文字（该函数把格式化的字符串写写入一个变量中，而不是输出来），其    他的与printf一样。
6. var_dump()：函数，输出变量的内容、类型或字符串的内容、类型、长度。常用来调试。

</details>



<details>
 <summary><b>不用新变量直接交换现有两个变量的值</b></summary>

方法一： 用list()
```
   a=10;b = 25;
   list(a,b)=array(b,a);
   echo a.′/′.b;//25/10
```
方法二：用传值方式加&符
```
  function swap(&a,&b)
  {
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
 <summary><b>heredoc</b></summary>

>Heredoc在正规的PHP文档中和技术书籍中一般没有详细讲述。他是一种Perl风格的字符串输出技术。使用heredoc技术可以实现界面与代码的准分离，比如phpwind模板。
heredoc的语法是用”<<<”加上自己定义成对的标签，在标签范围內的文字视为一个字符串

规则如下：

1. 以<<<End开始标记开始，以End结束标记结束，结束标记必须顶头写，不能有缩进和空格，且在结束标记末尾要有分号 。开始标记和开始标记相同，比如常用大写的EOT、EOD、EOF来表示，也可以使用其他标记，只要保证开始标记和结束标记不在正文中出现就行。

2. 位于开始标记和结束标记之间的变量可以被正常解析，但是函数则不可以。在heredoc中，变量不需要用连接符 . 或 , 来拼接，比如：
```
$a=2;
$b= <<<EOF
  "zyf"$a
  "zyf"
EOF;
echo $b; //结果连同双引号一起输出："zyf"2 "zyf"
```
3. heredoc常用在输出包含大量HTML语法文档的时候。他要比传统的echo输出精炼很多，如下所示：

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
 <summary><b>写个函数来解决多线程同时读写一个文件的问题</b></summary>

一般的方案：

```
//fopen():打开文件或者 URL，返回resource类型数据 。
$fp = fopen('./tmp/lock.txt', 'a+');
if (flock($fp, LOCK_EX)) {//取得独占锁定
    fwrite($fp, "Write something here\n");
    flock($fp, LOCK_UN);// 释放锁定
} else {
    echo "Couldn't lock the file !";
}
fclose($fp);
```
>但在PHP中，flock似乎工作的不是那么好！在多并发情况下，似乎是经常独占资源，不即时释放，或者是根本不释放，造成死锁，从而使服务器的cpu占用很高，甚至有时候会让服务器彻底死掉。

方案二：对文件进行加锁时，设置一个超时时间.
```
$fileName = './tmp/lock.txt';
if ($fp = fopen($fileName, 'a+')) {
    $startTime = microtime();
    while ((microtime() - $startTime) < 1000) {
        $canWrite = flock($fp, LOCK_EX);
        if (!$canWrite) {
            usleep(round(rand(0, 100) * 1000));
        } else {
            fwrite($fp, "Write something here\n");
            break;
        }
    }
    fclose($fp);
}
```
> 超时设置为1ms，如果这里时间内没有获得锁，就反复获得，直接获得到对文件操作权为止，当然。如果超时限制已到，就必需马上退出，让出锁让其它进程来进行操作。

</details>



<details>
 <summary><b>禁掉cookie的session使用方案，设置session过期的方法，对应函数</b></summary>

1. 设置php.ini的session.use_trans_sid = 1或者打开enable-trans-sid选项，让PHP自动跨页传递session id。
2. 手动通过URL传值, 隐藏表单传递session id。

```
  session_start();
  $_SESSION[’var1’]="源码爱好者";
  $sn = session_id();
  $url="<a href=".""s2.php?s=".$sn."">下一页</a>";
  echo $url;
  ```
  ```
  session_id($_GET[’s’]);
  session_start();
  echo "传递的session变量var1的值为：".$_SESSION[’var1’];
```

3. 用文件、数据库等形式保存session_id,在跨页过程中手动调用。
```
  $name=$_POST[’name’];
  $pass=$_POST[’pass’];

  session_start();
  $_SESSION[’user’]= $name;
  $psid=session_id();
  $fp=fopen("e:\tmp\phpsid.txt","w+";
  fwrite($fp,$psid);
  fclose($fp);
  echo "已登录<br>";
```
```
  $fp=fopen("e:\tmp\phpsid.txt","r";
  $sid=fread($fp,1024);
  fclose($fp);
  session_id($sid);
  session_start();
  if(isset($_SESSION[’user’]) && $_SESSION[’user’]="laogong" {
  echo "已登录!";
```

>Session过期时间设定:
```
ini_set('session.gc_maxlifetime', 3600); //设置时间 
ini_get('session.gc_maxlifetime');//得到ini中设定值 
```

</details>



<details>
 <summary><b>PHP魔术方法和魔术常量</b></summary>

##### 魔术常量

1. `__LINE__`
返回文件中的当前行号。
2. `__FILE__`
　　返回文件的完整路径和文件名。如果用在包含文件中，则返回包含文件名。自PHP4.0.2 起，__FILE__总是包含一个绝对路径，而在此之前的版本有时会包含一个相对路径。
3. `__FUNCTION__`
　　返回函数名称(PHP4.3.0 新加)。自PHP5 起本常量返回该函数被定义时的名字(区分大小写)。在PHP4 中该值总是小写字母的。
4. `__CLASS__`
　　返回类的名称(PHP4.3.0 新加)。自PHP5 起本常量返回该类被定义时的名字(区分大小写)。在PHP4 中该值总是小写字母的。
5. `__METHOD__`
　　返回类的方法名(PHP5.0.0 新加)。返回该方法被定义时的名字(区分大小写)。

##### Class魔术方法
>魔术方法是PHP面向对象中很有用特性。它们在特定的情况下被触发，都是以双下划线开头，可以理解为钩子函数，利用模式方法可以轻松实现PHP面向对象中重载（Overloading即动态创建类属性和方法）。魔术方法很多还是成对出现的，以下列出目前PHP中所有的模式方法。
1. __construct，__destruct
__constuct构建对象的时被调用；
__destruct明确销毁对象或脚本结束时被调用；
2. __get，__set
__set当给不可访问或不存在属性赋值时被调用
__get读取不可访问或不存在属性时被调用
3. __isset，__unset
__isset对不可访问或不存在的属性调用isset()或empty()时被调用
__unset对不可访问或不存在的属性进行unset时被调用
4. __call，__callStatic
__call调用不可访问或不存在的方法时被调用
__callStatic调用不可访问或不存在的静态方法时被调用
5. __sleep，__wakeup
__sleep当使用serialize时被调用，当你不需要保存大对象的所有数据时很有用
__wakeup当使用unserialize时被调用，可用于做些对象的初始化操作
6. __clone
进行对象clone时被调用，用来调整对象的克隆行为
7. __toString
当一个类被转换成字符串时被调用
8. __invoke
当以函数方式调用对象时被调用
9. __set_state
当调用var_export()导出类时，此静态方法被调用。用__set_state的返回值做为var_export的返回值。
10. __debuginfo
当调用var_dump()打印对象时被调用（当你不想打印所有属性）适用于PHP5.6版本

```
  class Animal{
  }
  $m = new Animal(); //__construct()被调用
  $m->not_exist_property = test; //__set()被调用
  echo $m->not_exist_property;//__get()被调用
  $m->abc(1,2,3); //__call()被调用
  echo isset($m->not_exist_property); //__isset()被调用，返回bool值
  unset($m->not_exist_property); //__unset()被调用
  echo $tmp = serialize($m); //__sleep()被调用
  unserialize($tmp); //__wakeup()被调用
  $m1 = clone $m; //__clone()被调用，对象默认是引用传递，使用clone关键词则可实现对象复制
  $m(); //__invoke() 已函数调用类
  eval( '$m2 = ' . var_export ( $m , true ) . ';' );
  var_dump($m2);
  var_dump($m);
  //最后__destruct()被调用
```

</details>


<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

```
function my_scandir($dir)
{
     $files = array();
     if ( $handle = opendir($dir) ) {
        //opendir() 函数打开一个目录句柄，可由 closedir()，readdir() 和 rewinddir() 使用。
         while ( ($file = readdir($handle)) !== false ) {
            //readdir() 函数返回由 opendir() 打开的目录句柄中的条目。
             if ( $file != ".." && $file != "." ) {
                 if ( is_dir($dir . "/" . $file) ) {  //测试文件是否为目录
                     $files[$file] = my_scandir($dir . "/" . $file);
                     //scandir() 函数返回一个数组，其中包含指定路径中的文件和目录。
                 }else {
                     $files[] = $file;
                 }
             }
         }
         closedir($handle);
         return $files;
     }
}
var_dump(my_scandir('php/ajax'));
```

</details>


<details>
 <summary><b>PHP缓存技术有哪些? tp是局部还是完全缓存?</b></summary>

1. 全页面静态化缓存：将页面全部生成为HTML静态页面，用户访问时直接访问静态页面，不走PHP服务器的解析流程。此种方式在CMS系统中比较常见，如dedecms。
实现方法：输出缓存
ob_start()--打开“输出控制缓冲”;
some code --要运行的代码;
$content=ob_get_contents()--返回“输出缓冲区的内容”;
some code --使用file_put_contents()等函数将返回的数据写入HTML文件;
ob_clearn()--清空“输出缓冲区”;

2. 页面部分缓存：将页面中不常变动的部分进行静态化缓存，而经常变化的部分不缓存，最后组装在一起显示；可以使用类似ob_get_contents()的方式实现，也可以利用类似ESI之类的页面片段缓存策略，使其用来做动态页面中相对静态的片段部分的缓存。该缓存方式常用与商城中的商品页。

3. 数据缓存：缓存数据的一种方式。比如，商城中的某个商品信息，当用商品id去请求时，就会得出包括店铺信息,商品信息等数据，此时就可以将这些 数据缓存到一个php文件中，文件名包含商品id来建一个唯一标示；下一次有人想查看这个商品时，首先就直接调这个文件里面的信息，而不用再去数据库查 询；其实缓存文件中缓存的就是一个php数组之类；Ecmall商城系统里面就用了这种方式；

4. 查询缓存：根据查询来进行缓存。将查询得到的数据缓存在一个文件中，下次遇到相同的查询时，就直接先从这个文件里面调数据，不再去查数据库；但此处的缓存文件名可能就需要以查询语句为基点来建立唯一标示.

>注意：以上几种缓存方式都需要对缓存的文件设置一个有效时间，在这个时间内，相同的访问会先取缓存文件的内容，超过有效时间后就重新从数据库中获取数据，并生成新的缓存文件。

5. 内存式缓存：使用redis,memcached等nosql数据库设置PHP缓存，通过缓存查询结果，来减少数据库的访问次数，从而提高网站的响应速度。

6. pache缓存模块：apache安装完以后，是不允许被cache的。如果外接了cache或squid服务器要求进行web加速的话，就需要在htttpd.conf里进行设置，当然前提是在安装apache的时候要激活mod_cache的模块。
安装apache时：./configure –enable-cache –enable-disk-cache –enable-mem-cache

7. PHP APC缓存扩展：使用PHP中的APC扩展来进行缓存

8. Opcode：首先php代码被解析为Tokens，然后再编译为Opcode码，最后执行Opcode码，返回结果；所以，对于相同的php文件，第一次运行时 可以缓存其Opcode码，下次再执行这个页面时，直接会去找到缓存下的opcode码，直接执行最后一步，而不再需要中间的步骤了。比较知名的是XCache、Turck MM Cache、PHP Accelerator等。关于PHP Opcode，鸟哥的博客中有一篇详解：http://www.laruence.com/2008/06/18/221.html

</details>



<details>
 <summary><b>strlen()与mb_strlen的作用与区别</b></summary>

在PHP中，strlen与mb_strlen是求字符串长度的函数
PHP内置的字符串长度函数strlen无法正确处理中文字符串，它得到的只是字符串所占的字节数。对于GB2312的中文编码，strlen得到的值是汉字个数的2倍，而对于UTF-8编码的中文，就是3倍（在 UTF-8编码下，一个汉字占3个字节）。

采用mb_strlen函数可以较好地解决这个问题。mb_strlen的用法和strlen类似，只不过它有第二个可选参数用于指定字符编码。例如得到UTF-8的字符串str长度，可以用mbstrlen(str长度，可以用mbstrlen(str,‘UTF-8’)。如果省略第二个参数，则会使用PHP的内部编码。内部编码可以通过 mb_internal_encoding()函数得到。

需要注意的是，mb_strlen并不是PHP核心函数，使用前需要确保在php.ini中加载了php_mbstring.dll，即确保“extension=php_mbstring.dll”这一行存在并且没有被注释掉，否则会出现未定义函 数的问题。

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>



<details>
 <summary><b>写一个函数，可以遍历文件夹下的所有文件和文件夹</b></summary>

 

</details>