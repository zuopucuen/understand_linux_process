
## 文件描述符

Linux很重要的设计思想就是一切皆文件，网络是文件，键盘等外设也是文件，很神奇吧？于是所有资源都有了统一的接口，开发者可以像写文件那样通过网络传输数据，我们也可以通过`/proc/`的文件看到进程的资源使用情况。

内核给每个访问的文件分配了文件描述符(File Descriptor)，它本质是一个非负整数，在打开或新建文件时返回，以后读写文件都要通过这个文件描述符了。

## 应用

我们想想操作系统打开的文件这么多，不可能他们共用一套文件描述符整数吧？这样想就对了，Linux实现时这个fd其实是一个索引值，指向每个进程打开文件的记录表。

POSIX已经定义了STDIN_FILENO、STDOUT_FILENO和STDERR_FILENO三个常量，也就是0、1、2。这三个文件描述符是每个进程都有的，这也解释了为什么每个进程都有编号为0、1、2的文件而不会与其他进程冲突。

文件描述符帮助应用找到这个文件，而文件的打开模式等上下文信息存储在文件对象中，这个对象直接与文件描述符关联。

## 限制

注意了，每个系统对文件描述符个数都有限制。我们网上看到配置`ulimit`也是为了调大系统的打开文件个数，因为一般服务器都要同时处理成千上万个起请求，记住socket连接也是文件哦，使用系统默认值会出现莫名奇怪的问题。

讲文件描述符其实是为高深莫测的epoll做铺垫，掌握epoll对进程已经有很深的理解了。