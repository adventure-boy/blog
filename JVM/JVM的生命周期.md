# JVM的生命周期

包括了启动，运行，结束

## 启动

​	Java虚拟机的启动是引导类加载器（bootstarp class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的

## 运行

1. Java虚拟机的运行就是为了执行java程序
2. 程序执行JVM就运行，程序结束它就结束
3. 执行的java程序本质就是一个java虚拟机进程，可通过命令jps查看             

## 结束

1. 程序正常结束
2. 执行过程中遇到异常或错误而导致异常终止
3. 操作系统出错导致java虚拟机进程终止
4. 某线程调用Runtime类或者System类的exit方法，或Runtime的halt方法，并且Java安全管理器也允许exit或者halt操作。