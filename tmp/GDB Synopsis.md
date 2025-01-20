# 简要操作步骤

1.  解除core文件的大小生成限制

   ~~~bash
   ulimit -c unlimited
   ~~~

2.  指定core文件的生成路径

   ~~~bash
   sudo sh -c "echo "path/filename_%u_%g_%t" > /proc/sys/kernel/core_pattern"
   ~~~

3.  修改CMakeLists.txt，增加调试信息

   ~~~cmake
   add_definitions("-g")
   ~~~

4.  调试core文件

   ~~~bash
   gdb path/executable path/corefile
   ~~~
