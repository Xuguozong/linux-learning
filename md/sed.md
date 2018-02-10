### sed工作流

 * 读取：sed从输入流(文件、管道或stdin)中读取一行数据并存储到模式空间的内部缓冲区
 * 执行：除非指定行地址，sed命令依次在模式空间中顺序执行
 * 显示：发送修改后内容到输出流，并清空模式空间
 
 ### 基础语法
 
     sed [-n] [-e] 'commands' files
     sed [-n] -f scriptfile files
     # 
     -n 参数表示不输出到标准输出
     -e 指定要执行的命令，可以有多个命令
     
 ### p命令
     
     # 输出file文件内容
     sed -n 'p' file
     
     # 只输出第三行
     sed -n '3p' file
     
     # 输出2到5行
     sed -n '2,5 p' file
     
     # 输出最后一行
     sed -n '$ p' file
     
     # 输出2到最后一行
     sed -n '2,$ p' file
     
     # M,+n M行的下n行
     sed -n '2,+4 p' file
     
     # ~ M行开始的每N行
     sed -n '1~2 p' file

### 文本模式过滤器
     
     