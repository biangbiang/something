使用 Python 创建 UNIX 命令行工具
================================

### Python 命令行接口声明

如果您在 IT 部门担任 UNIX® 系统管理员、软件开发人员甚至是经理，掌握几项技能将使您与众不同。您是否对 OSI 模型有充分了解？您对子网划分是否得心应手？您了解 UNXI 权限吗？让我为您的技能背景增添一个不起眼的命令行工具。在本文结束时，在 IT 部门担任任何职位的读者都应该至少能创建一个简单的命令行工具。

---

### 引言

您是否能编写命令行工具？也许您可以，但您能编写出真正好用的命令行工具吗？本文讨论使用 Python 来创建一个强健的命令行工具，并带有内置的帮助菜单、错误处理和选项处理。由于一些奇怪的原因，很多人并不了解 Python® 的标准库具有制作功能极其强大的 *NIX 命令行工具所需的全部工具。

可以这样说，Python 是制作 *NIX 命令行工具的最佳语言，因为它依照“batteries-included”的哲学方式工作，并且强调提供可读性高的代码。但仅作为提醒，当您发现使用 Python 创建命令行工具是一件多么简单的事情时，这些想法很危险，您的生活可能被搅得一团糟。据我所知，至今还没有发表过详细说明使用 Python 创建命令行工具的文章，因此我希望您喜欢这篇文章。

### 设置

Python 标准库中的 optparse 模块可完成创建命令行工具的大部分琐碎工作。optparse 包含在 Python 2.3 中，因此该模块将包括在许多 *NIX 操作系统中。如果由于某种原因，您使用的操作系统不包含所需要的模块，那么值得庆幸的是，Python 的最新版本已经过测试并编译到几乎任何 *NIX 操作系统中。Python 支持的系统包括 IBM® AIX®、HP-UX、Solaris、Free BSD、Red Hat Linux®、Ubuntu、OS X、IRIX，甚至包括几种 Nokia 手机。

### 创建 Hello World 命令行工具

编写优秀的命令行工具的第一步是定义要解决的问题。这对您工具的成功至关重要。这对于以尽可能简单的方法解决问题也同样重要。这里明确地采用了 KISS（Keep It Simple Stupid，保持简单）准则。只有在实现并测试了计划内功能之后才添加选项和增加其他功能。

我们首先从创建 Hello World 命令行工具开始。按照上面的建议，我们使用尽可能简单的术语来定义问题。

问题定义：我希望创建一个命令行工具，默认打印 Hello World，并提供用于打印不通人的姓名的选项。

基于上述说明，可以提供一个包含少量代码的解决方案。

Hello World 命令行接口 (CLI)

    #!/usr/bin/env python
    import optparse

    def main():
      p = optparse.OptionParser()
      p.add_option('--person', '-p', default="world")
      options, arguments = p.parse_args()
      print 'Hello %s' % options.person

    if __name__ == '__main__':
      main()

如果运行此代码，预期的输出如下：

    Hello world

但是，我们通过少量代码所能做到的远不止于此。我们可以获得自动生成的帮助菜单：

    python hello_cli.py --help   
              Usage: hello_cli.py [options]
              
              Options:
              -h, --help            show this help message and exit
              -p PERSON, --person=PERSON

从帮助菜单中可以了解到，我们可以使用两种方法来更改 Hello World 的输出：

    python hello_cli.py -p guido
    Hello guido

我们还实现了自动生成的错误处理：

    python hello_cli.py --name matz
    Usage: hello_cli.py [options]
              
    hello_cli.py: error: no such option: --name

如果您还没有使用过 Python 的 optparse 模块，那么您刚才可能会大吃一惊，并思忖使用 Python 可以编写的所有这些不可思议的工具。如果您刚开始接触 Python，那么您可能会惊讶于 Python 让一切变得如此简单。“XKCD”网站发表了关于“Python 是如此简单”主题的非常有趣的漫画，已包括在参考资料中。

---

### 创建有用的命令行工具

既然我们已经打好了基础，我们就可以继续创建解决特定问题的工具。对于本例，我们将使用 Python 的名为 Scapy 的网络库和交互式工具。Scapy 可以在大多数 *NIX 系统上正常工作，可以在第 2 层和第 3 层上发送数据包，并允许您创建只有几行 Python 代码的非常复杂的工具。如果您希望按部就班从头开始，请确保您正确地安装了必要的软件。

我们先定义要解决的新问题。

问题：我希望创建一个使用 IP 地址或子网作为参数的命令行工具，并向标准输出返回 MAC 地址或 MAC 地址列表以及它们各自的 IP 地址。

既然我们已经清楚地定义了问题，让我尝试将问题分解为尽可能简单的部分，然后逐一解决这些部分。对于这一问题，我看到了两个独立的部分。第一部分是编写接收 IP 地址或子网范围的函数，并返回 MAC 地址或 MAC 地址列表。我们可以在解决此问题之后再考虑将其集成到命令行工具中。

### 解决方案第 1 部分：创建通过 IP 地址确定 MAC 地址的 Python 函数

arping

    from scapy import srp,Ether,ARP,conf

    conf.verb=0
    ans,unans=srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst="10.0.1.1"),
    timeout=2)

    for snd, rcv in ans:
    print rcv.sprintf(r"%Ether.src% %ARP.psrc%")

该命令的输出是：

    sudo python arping.py
    00:00:00:00:00:01 10.0.1.1

请注意，使用 scapy 执行操作要求提升的权限，因此我们必须使用 sudo。考虑到本文的目的，我还将实际输出更改为包括伪 MAC 地址。我们已经证实了我们可以通过 IP 地址找到 MAC 地址。我们需要整理此代码以接受 IP 地址或子网并返回 MAC 地址和 IP 地址对。

arping 函数

    #!/usr/bin/env python

    from scapy import srp,Ether,ARP,conf

    def arping(iprange="10.0.1.0/24"):
      conf.verb=0
      ans,unans=srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=iprange),
                          timeout=2)

      collection = []
      for snd, rcv in ans:
        result = rcv.sprintf(r"%ARP.psrc% %Ether.src%").split()
        collection.append(result)
      return collection

    #Print results
    values = arping()

    for ip,mac in values:
    print ip,mac

正如您看到的，我们编写了一个函数，该函数接受 IP 地址或网络并返回嵌套的 IP/MAC 地址列表。我们现已为第二部分做好准备，为我们的工具创建一个命令行接口。

### 解决方案第 2 部分：从我们的 arping 函数创建命令行工具

在本例中，我们综合本文前面部分的想法，创建一个能解决我们初始问题的完整命令行工具。

##### arping CLI

    #!/usr/bin/env python

    import optparse
    from scapy import srp,Ether,ARP,conf

    def arping(iprange="10.0.1.0/24"):
      """Arping function takes IP Address or Network, returns nested mac/ip list"""

      conf.verb=0
      ans,unans=srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=iprange),
                timeout=2)

      collection = []
      for snd, rcv in ans:
          result = rcv.sprintf(r"%ARP.psrc% %Ether.src%").split()
          collection.append(result)
      return collection

    def main():
      """Runs program and handles command line options"""

      p = optparse.OptionParser(description=' Finds MAC Address of IP address(es)',
                                    prog='pyarping',
                                    version='pyarping 0.1',
                                    usage='%prog [10.0.1.1 or 10.0.1.0/24]')

    options, arguments = p.parse_args()
    if len(arguments) == 1:
      values = arping(iprange=arguments)
      for ip, mac in values:
        print ip, mac
    else:
      p.print_help()

    if __name__ == '__main__':
      main()

对以上脚本进行几点说明将有助于我们了解 optparse 的工作方式。

首先，必须创建 optparse.OptionParser() 的一个实例，并且接受如下所示的可选参数：

      description, prog, version, and usage

这些参数的含义基本上可以不言自明，但我希望确认的一点是，您应该了解 optparse 虽然功能强大，但并不是无所不能。它具有明确定义的接口，可用于快速创建命令行工具。

其次，在如下行中：

    options, arguments = p.parse_args()

该行的作用是将选项和参数划分为不同的位。在上述代码中，我们预期恰有一个参数，因此我指定必须只有一个参数值，并将该值传递给 arping 函数。

    if len(arguments) == 1:
      values = arping(iprange=arguments)

为了进一步说明，让我们运行下面的命令以了解其工作方式：

    sudo python arping.py 10.0.1.1

    10.0.1.1 00:00:00:00:00:01

在上述示例中，参数为 10.0.1.1，由于正如我在条件语句中指定的那样只有一个参数，因此该参数被传递给 arping 函数。如果存在选项，它们将在 options, arguments = p.parse_args() 方法中传递给 options。让我们看一下，当我们分解命令行工具的预期用例并赋予该用例两个参数时将会发生什么情况：

    sudo python arping.py 10.0.1.1 10.0.1.3
    Usage: pyarping [10.0.1.1 or 10.0.1.0/24]

    Finds MAC Address or IP address(es)

    Options:
    --version   show program's version number and exit
    -h, --help  show this help message and exit

根据我为参数构建的条件语句的结构，如果参数的数目不为 1，它将自动打开帮助菜单：

if len(arguments) == 1:
  values = arping(iprange=arguments)
  for ip, mac in values:
    print ip, mac
else:
  p.print_help()

这是一种用于控制工具的工作方式的重要方法，因为您可以使用参数的个数或特定选项的名称作为控制命令行工具的流程的机制。因为我们在最初的 Hello World 示例中涉及了选项的创建，接下来通过略微更改主函数向我们的命令行工具添加几个选项：

##### arping CLI main 函数

    def main():
      """Runs program and handles command line options"""

      p = optparse.OptionParser(description='Finds MAC Address of IP address(es)',
                                prog='pyarping',
                                version='pyarping 0.1',
                                usage='%prog [10.0.1.1 or 10.0.1.0/24]')
      p.add_option('-m', '--mac', action ='store_true', help='returns only mac address')
      p.add_option('-v', '--verbose', action ='store_true', help='returns verbose output')

      options, arguments = p.parse_args()
      if len(arguments) == 1:
        values = arping(iprange=arguments)
        if options.mac:
          for ip, mac in values:
            print mac
        elif options.verbose:
          for ip, mac in values:
            print "IP: %s MAC: %s " % (ip, mac)
        else:
          for ip, mac in values:
            print ip, mac

      else:
        p.print_help()

所做的主要更改是创建了基于是否指定了某个选项的条件语句。请注意，与 Hello World 命令行工具不同，我们仅使用选项作为我们工具的 true/false 信号。对于 –MAC 选项的情况，如果指定了该选项，我们的条件语句 elif 将只打印 MAC 地址。

下面是新选项的输出：

##### arping 输出

    sudo python arping2.py 
    Password:
    Usage: pyarping [10.0.1.1 or 10.0.1.0/24]

    Finds MAC Address of IP address(es)

    Options:
    --version      show program's version number and exit
    -h, --help     show this help message and exit
    -m, --mac      returns only mac address
    -v, --verbose  returns verbose output
    [ngift@M-6][H:11184][J:0]> sudo python arping2.py 10.0.1.1
    10.0.1.1 00:00:00:00:00:01
    [ngift@M-6][H:11185][J:0]> sudo python arping2.py -m 10.0.1.1
    00:00:00:00:00:01
    [ngift@M-6][H:11186][J:0]> sudo python arping2.py -v 10.0.1.1
    IP: 10.0.1.1 MAC: 00:00:00:00:00:01

---

### 深入学习创建命令行工具

下面是几个用于深入学习的新想法。在我正与别人合著的有关 Python *NIX 系统管理的书中对这些想法进行了深入的探讨，该书将在 2008 年中期出版。

### 在命令行工具中使用 subprocess 模块

subprocess 模块包括在 Python 2.4 或更高版本中，是用于处理系统调用和流程的统一接口。您可以轻松替换上面的 arping 函数，以使用适用于您的特定 *NIX 操作系统的 arping 工具。以下是体现上述想法的粗略示例：

##### 子流程 arping

    import subprocess
    import re
    def arping(ipaddress="10.0.1.1"):
      """Arping function takes IP Address or Network, returns nested mac/ip list"""
    
      #Assuming use of arping on Red Hat Linux
      p = subprocess.Popen("/usr/sbin/arping -c 2 %s" % ipaddress, shell=True,
                            stdout=subprocess.PIPE)
      out = p.stdout.read()
      result = out.split()
      pattern = re.compile(":")
      for item in result:
        if re.search(pattern, item):
          print item
    arping()

以下是该函数单独运行时的输出： `[root@localhost]~# python pyarp.py [00:16:CB:C3:B4:10]`

请注意使用 subprocess 来获取 arping 命令的输出，以及使用已编译的正则表达式匹配 MAC 地址。注意，如果您使用的是 Python 2.3，则可以使用 popen 模块替换 subprocess，后者在 Python 2.4 或更高版本中提供。

### 在命令行工具中使用对象关系映射器，如配合 SQLite 使用的 SQLAlchemy 或 Storm

命令行工具的另一个可能选项是使用 ORM（对象关系映射器）来存储由命令行工具生成的数据记录。有相当多的 ORM 可用于 Python，但 SQLAlchemy 和 Storm 恰好是最常用的两个。我通过掷硬币的方式决定使用 Storm 作为示例：

##### Storm ORM arping

    #!/usr/bin/env python
    import optparse
    from storm.locals import *
    from scapy import srp,Ether,ARP,conf

    class NetworkRecord(object):
      __storm_table__ = "networkrecord"
      id = Int(primary=True)
      ip = RawStr()
      mac = RawStr()
      hostname = RawStr()

    def arping(iprange="10.0.1.0/24"):
      """Arping function takes IP Address or Network, 
      returns nested mac/ip list"""

      conf.verb=0
      ans,unans=srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=iprange),
                    timeout=2)
      collection = []
      for snd, rcv in ans:
        result = rcv.sprintf(r"%ARP.psrc% %Ether.src%").split()
        collection.append(result)
      return collection

    def main():
      """Runs program and handles command line options"""

      p = optparse.OptionParser()
      p = optparse.OptionParser(description='Finds MACAddr of IP address(es)',
                                prog='pyarping',
                                version='pyarping 0.1',
                                usage= '%prog [10.0.1.1 or 10.0.1.0/24]')

      options, arguments = p.parse_args()
      if len(arguments) == 1:
        database = create_database("sqlite:")
        store = Store(database)
        store.execute("CREATE TABLE networkrecord "
              "(id INTEGER PRIMARY KEY, ip VARCHAR,\ 
              mac VARCHAR, hostname VARCHAR)")
        values = arping(iprange=arguments)
        machine = NetworkRecord()
        store.add(machine)
        #Creates Records
        for ip, mac in values:
          machine.mac = mac
          machine.ip = ip
        #Flushes to database
        store.flush()
        #Prints Record
        print "Record Number: %r" % machine.id
        print "MAC Address: %r" % machine.mac
        print "IP Address: %r" % machine.ip
      else:
        p.print_help()

    if __name__ == '__main__':
      main()

本例中需要关注的主要内容是创建名为 NetworkRecord 的类，该类映射到“内存中”的 SQLite 数据库。在 main 函数中，我将 arping 函数的输出更改为映射到我们的记录对象，将它们更新到数据库，然后再将其取回以打印结果。这明显不是一个可用于生产的工具，但可作为在我们的工具中使用 ORM 的相关步骤的说明性示例。

### 在 CLI 中集成 config 文件

##### Python INI config 语法

    [AIX]
    MAC: 00:00:00:00:02
    IP: 10.0.1.2
    Hostname: aix.example.com
    [HPUX]
    MAC: 00:00:00:00:03
    IP: 10.0.1.3
    Hostname: hpux.example.com
    [SOLARIS]
    MAC: 00:00:00:00:04
    IP: 10.0.1.4
    Hostname: solaris.example.com
    [REDHAT]
    MAC: 00:00:00:00:05
    IP: 10.0.1.5
    Hostname: redhat.example.com
    [UBUNTU]
    MAC: 00:00:00:00:06
    IP: 10.0.1.6
    Hostname: ubuntu.example.com
    [OSX]
    MAC: 00:00:00:00:07
    IP: 10.0.1.7
    Hostname: osx.example.com

接下来，我们需要使用 ConfigParser 模块来解析上述内容：

##### ConfigParser 函数

    #!/usr/bin/env python
    import ConfigParser

    def readConfig(file="config.ini"):
      Config = ConfigParser.ConfigParser()
      Config.read(file)
      sections = Config.sections()
      for machine in sections:
        #uncomment line below to see how this config file is parsed
        #print Config.items(machine)
        macAddr = Config.items(machine)[0][1]
        print machine, macAddr
    readConfig()

该函数的输出如下：

    OSX 00:00:00:00:07
    SOLARIS 00:00:00:00:04
    AIX 00:00:00:00:02
    REDHAT 00:00:00:00:05
    UBUNTU 00:00:00:00:06
    HPUX 00:00:00:00:03

我将剩下的问题作为练习留给读者来解决。我接下来要做的是将该 config 文件集成到我的脚本中，这样我就可以将我的 config 文件中记录的机器库存与出现在 ARP 缓存中的 MAC 地址的实际库存进行比较。IP 地址或主机名只在跟踪到计算机时才能发挥其作用，但是我们实现的工具对于跟踪网络上存在的计算机的硬件地址并确定它以前是否出现在网络上可能非常有用。

---

### 结束语

我们首先通过编写几行代码创建了一个非常简单但功能强大的 Hello World 命令行工具。然后使用 Python 网络库创建了一个复杂的网络工具。最后，我们继续讨论一些更高级的研究领域以飨读者。在高级研究部分，我们讨论了 subprocess 模块、对象关系映射器的集成，最后讨论了配置文件。

虽然并不为众人所知，但任何具有 IT 背景的读者都可以使用 Python 轻松地创建命令行工具。我希望本文能够激励您亲自动手创建全新的命令行工具。
