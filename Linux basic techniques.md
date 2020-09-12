<details>
 <summary><b>linux中怎么查看系统资源占用情况,同时会问到怎么查看TCP端口,TCP连接状态</b></summary>

top         //可能会问得更深,比如显示出来有哪些信息、你关心哪些信息、查看某个进程等
iostat      //磁盘cpu
free        //内存剩余
df          //磁盘使用情况
du          //文件占用信息

ps              //查看进程信息
netstat -anptol //查看端口占用情况,参数细节建议查文档,小心被问倒

</details>


1. Core文件是什么，有什么用？
2. 共享内存除了文件映射还有什么方式？
3. 请解释下列10个shell命令的用途
4. Linux文件属性有哪些？（共十位）
5. Linux查询命令