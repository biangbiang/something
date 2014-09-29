Mac系统下的launchd
================

Mac系统下通用的进程管理器，是Mac系统下非常重要的一个进程。一般来说该进程不允许直接以命令行的形式调用。只能通过其控制管理界面，launchctl来进行控制。

launchd主要功能是进程管理。可以理解成是一个常驻在后台的进程，根据用户的配置，来响应特定的系统事件。launchd既可以用于系统级别的服务，又可以用于个人用户级别的服务。

在launchd的语境中，常驻进程有两种，

* 一种称为是daemon，也就是我们常说的守护进程，这种一般对所有用户都有相同的行为，响应相同的事件，始终运行于后台，没有前台交互界面。

* 另一种称为是agent，这种是用户级别的服务进程，一般以用户的身份运行。

~/Library/LaunchAgents 用户的进程

/Library/LaunchAgents 管理员设置的用户进程

/Library/LaunchDaemons 管理员提供的系统守护进程

/System/Library/LaunchAgents Mac操作系统提供的用户进程

/System/Library/LaunchDaemons Mac操作系统提供的系统守护进程

以上是launchd的相关配置的存放目录，可以看到，一般我们个人编写的守护进程，都应该放到~/Library/LaunchAgents目录里面。
