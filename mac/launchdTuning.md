Mac OS启动服务优化高级篇（launchd tuning）
=====================================

Mac下的启动服务主要有三个地方可配置：

1. 系统偏好设置->帐户->登陆项
2. /System/Library/StartupItems 和 /Library/StartupItems/
3. launchd 系统初始化进程配置。

前两种优化比较简单，本文主要介绍的是第三种更为复杂的launchd配置优化。

launchd是Mac OS下，用于初始化系统环境的关键进程。类似Linux下的init, rc。

我们先来看一下Mac OS X的启动原理：

1. mac固件激活，初始化硬件，加载BootX引导器。
2. BootX加载内核与内核扩展(kext)。
3. 内核启动launchd进程。
4. launchd根据 /System/Library/LaunchAgents ,/System/Library/LaunchDaemons , /Library/LaunchDaemons,/Library/LaunchAgents , ~/Library/LaunchAgents 里的plist配置，启动服务守护进程。

看完了Mac OS X的启动原理，我们不难发觉/System/Library/LaunchAgents ,/System/Library/LaunchDaemons , /Library/LaunchDaemons,/Library/LaunchAgents , ~/Library/LaunchAgents 五个目录下的plist属性文件是优化系统的关键。

下面再来理解几个基础概念：

* /System/Library和/Library和~/Library目录的区别？
* /System/Library目录是存放Apple自己开发的软件。
* /Library目录是系统管理员存放的第三方软件。
* ~/Library/是用户自己存放的第三方软件。

LaunchDaemons和LaunchAgents的区别？

* LaunchDaemons是用户未登陆前就启动的服务（守护进程）。
* LaunchAgents是用户登陆后启动的服务（守护进程）。

上面提到的五个目录下的plist文件格式及每个字段的含义：

    Key	Description	Required
    Label	The name of the job	yes
    ProgramArguments	Strings to pass to the program when it is executed	yes
    UserName	The job will be run as the given user, who may not necessarily be the one who submitted it to launchd.	no
    inetdCompatibility	Indicates that the daemon expects to be run as if it were launched by?inetd	no
    Program	The path to your executable. This key can save the ProgramArguments key for flags and arguments.	no
    onDemand	A?boolean?flag that defines if a job runs continuously or not	no
    RootDirectory	The job will be?chrooted?into another directory	no
    ServiceIPC	Whether the daemon can speak IPC to launchd	no
    WatchPaths	Allows launchd to start a job based on modifications at a file-system path	no
    QueueDirectories	Similar to WatchPath, a queue will only watch an empty directory for new files	no
    StartInterval	Used to schedule a job that runs on a repeating schedule. Specified as the number of seconds to wait between runs.	no
    StartCalendarInterval	Job scheduling. The?syntax?is similar to?cron.	no
    HardResourceLimits	Controls restriction of the resources consumed by any job	no
    LowPriorityIO	Tells the kernel that this task is of a low priority when doing file system I/O	no
    Sockets	An array can be used to specify what socket the daemon will listen on for launch on demand	no

看不懂上面地plist配置吗？没关系，我们的优化策略是完全卸载服务，所以我们不用关心plist里的配置含义。

## 开始优化

禁用服务，我们需要用到Mac OS提供的一个工具指令－launchctl

launchctl 指令会针对服务设置一个禁用标志，launchd启动时会先检查这个服务是否被禁用，从而确定是否需要启用这个服务。

### 禁用服务的方法1

先找到禁用标志文件 /var/db/launchd.db/com.apple.launchd/overrides.plist，查看你要禁用的服务是否已被禁用了。

有些服务已被禁用，但未列在overrides.plist里。此时，你还需要检查这个服务的plist文件Label字段是否已经标记为 Disable。

确认这个服务未禁用后，我们就可以通过调用如下命令，来禁用服务：

    sudo launchctl unload plist文件路径
    sudo launchctl unload -w?plist文件路径

比如，我想禁用spotlight，则输入

    sudo launchctl unload?/System/Library/LaunchAgents/com.apple.Spotlight.plist
    sudo launchctl unload -w?/System/Library/LaunchAgents/com.apple.Spotlight.plist

禁用完服务以后，重启Mac OS即可生效。

### 禁用服务的方法2，一种更有效且暴力的方法（推荐）

先卸载服务

    sudo launchctl unload /System/Library/LaunchAgents/com.apple.Spotlight.plist

然后将plist文件mv到其他目录备份。重启。搞定。是不是很简单!

我个人比较喜欢这种禁用服务的方式，所以推荐一下。

### 如果发现服务禁用后，系统或软件出现异常，可以通过如下命令，还原服务：

方法1:

    sudo launchctl load -wF?plist文件路径

方法2:

将备份的plist文件mv回原来的文件夹。

    sudo launchctl load plist文件路径

注意：系统级服务的禁用要异常小心，请在禁用前google，确保你熟知这个服务的作用。否则可能导致系统无法启动。

最安全的做法就是不要去禁用它了。

当然，用户服务我们还是可以放心禁用的，有问题最多再启用呗。

### 下面是我禁用的服务列表：

    /System/Library/LaunchDaemons/com.apple.metadata.mds.plist (禁用spotlight的前提)
    /System/Library/LaunchAgents/com.apple.Spotlight.plist (Spotlight)
    /Library/LaunchDaemons/com.google.keystone.daemon.plist ?(Google Software Update)
    /Library/LaunchAgents/com.google.keystone.root.agent ?(Google Software Update)
    ~/Library/LaunchAgents/com.google.keystone.agent.plist?(Google Software Update，用户下的进程不需要加 sudo)
    ~/Library/LaunchAgents/com.apple.CSConfigDotMacCert-ken.wug\@me.com-SharedServices.Agent.plist (me.com的共享服务，我不用)
    /System/Library/LaunchDaemons/org.cups.cupsd.plist （打印机）
    /System/Library/LaunchDaemons/org.cups.cups-lpd.plist （打印机）
    /System/Library/LaunchDaemons/com.apple.blued.plist （蓝牙）
    /System/Library/LaunchAgents/com.apple.AirPortBaseStationAgent.plist （apple无线基站，我没有这个设备）

### 知道守护进程（服务）名，如何找到对应的plist文件？

将进程（服务）名拷贝，然后到 /System/Library/LaunchAgents ,/System/Library/LaunchDaemons , /Library/LaunchDaemons,/Library/LaunchAgents , ~/Library/LaunchAgents 五个目录里，通过以下命令查找：

ll|grep 进程(服务)名

比如

    ll|grep blued

在 /System/Library/LaunchDaemons 中找到了它。接下来，请按上面指导的步骤，禁用该服务。
