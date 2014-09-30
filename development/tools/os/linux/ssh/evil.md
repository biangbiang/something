VPS防止SSH暴力登录尝试攻击
======================

前些时谈了一下如何屏蔽对网站服务器的扫描，属于前台防御。后来 Felix 发了一篇 blog提到将多次尝试 SSH 登录失败的 IP ban 掉，才想起来去看一下日志，没想到后院起火了。

查看日志文件：

    $ sudo cat /var/log/auth.log

没想到满屏满屏的往下刷，全是

    Failed password for root from 123.15.36.218 port 51252 ssh2
    reverse mapping checking getaddrinfo for pc0.zz.ha.cn [218.28.79.228] failed – POSSIBLE BREAK-IN ATTEMPT!
    Invalid user akkermans from 218.28.79.228
    pam_unix(sshd:auth): check pass; user unknown
    pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=218.28.79.228

来统计一下有多少人在暴力破解我的 root 密码吧

    $ sudo grep "Failed password for root" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | more

    470 222.122.52.150
    411 123.15.36.218
    139 177.8.168.48
     20 74.81.83.226
     18 77.108.112.131
      2 95.58.255.62
      1 218.28.79.228
      1 188.132.163.154
很明显我禁用了 root 登录，人家也不是那么笨，开始暴力猜用户名

    $ sudo grep "Failed password for invalid user" /var/log/auth.log | awk '{print $13}' | sort | uniq -c | sort -nr | more

    3190 218.28.79.228
     646 222.122.52.150
     172 123.15.36.218
      65 177.8.168.48
       4 222.76.211.149

某个人尝试了 3000 多次，好吧，这个小博客真有那么 valuable 么。。为了防范于未然，我们可以做些配置，让 VPS 服务器更加安全。

##### 1、修改 SSH 端口，禁止 root 登陆

修改/etc/ssh/sshd_config文件

    $ sudo vi /etc/ssh/sshd_config

    Port 4484 #一个别人猜不到的端口号
    PermitRootLogin no

    $ sudo /etc/init.d/ssh restart

##### 2、禁用密码登陆，使用 RSA 私钥登录

Amazon EC2 服务器本来就是只允许使用私钥登录的，但是这样的话我如果想在别的电脑上临时 SSH 上来，又没带私钥文件的情况下，就很麻烦。所以我又手动开启了密码验证登录。不管怎样，这一条还是先列出来吧

    # 在客户端生成密钥
    $ ssh-keygen -t rsa
    # 把公钥拷贝至服务器
    $ ssh-copy-id -i .ssh/id_rsa.pub server
    # 也可以手动将.shh/id_rsa.pub拷贝至服务器用户目录的.ssh中，记得修改访问权限
    # $ scp .shh/id_rsa.pub server:~/.ssh
    # 在服务器中
    $ cd ./.ssh/
    $ mv id_rsa.pub authorized_keys
    $ chmod 400 authorized_keys
    $ vi /etc/ssh/sshd_config
    RSAAuthentication yes #RSA认证
    PubkeyAuthentication yes #开启公钥验证
    AuthorizedKeysFile .ssh/authorized_keys #验证文件路径
    PasswordAuthentication no #禁止密码认证
    PermitEmptyPasswords no #禁止空密码
    UsePAM no #禁用PAM

    # 最后保存，重启
    $ sudo /etc/init.d/ssh restart

##### 3、安装denyhosts

这个方法比较省时省力。denyhosts 是 Python 语言写的一个程序，它会分析 sshd 的日志文件，当发现重复的失败登录时就会记录 IP 到 /etc/hosts.deny 文件，从而达到自动屏 IP 的功能。这和我之前介绍的自动屏蔽扫描的脚本是一个思路。如果靠人工手动添加的话还不把人累死。现今 denyhosts 在各个发行版软件仓库里都有，而且也不需要过多配置，傻瓜易用。

安装：

    # Debian/Ubuntu：
    $ sudo apt-get install denyhosts

    # RedHat/CentOS
    $ yum install denyhosts

    # Archlinux
    $ yaourt denyhosts

    # Gentoo
    $ emerge -av denyhosts
    默认配置就能很好的工作，如要个性化设置可以修改 /etc/denyhosts.conf

    $ vi /etc/denyhosts.conf
    SECURE_LOG = /var/log/auth.log #ssh 日志文件，它是根据这个文件来判断的。
    HOSTS_DENY = /etc/hosts.deny #控制用户登陆的文件
    PURGE_DENY = #过多久后清除已经禁止的，空表示永远不解禁
    BLOCK_SERVICE = sshd #禁止的服务名，如还要添加其他服务，只需添加逗号跟上相应的服务即可
    DENY_THRESHOLD_INVALID = 5 #允许无效用户失败的次数
    DENY_THRESHOLD_VALID = 10 #允许普通用户登陆失败的次数
    DENY_THRESHOLD_ROOT = 1 #允许root登陆失败的次数
    DENY_THRESHOLD_RESTRICTED = 1
    WORK_DIR = /var/lib/denyhosts #运行目录
    SUSPICIOUS_LOGIN_REPORT_ALLOWED_HOSTS=YES
    HOSTNAME_LOOKUP=YES #是否进行域名反解析
    LOCK_FILE = /var/run/denyhosts.pid #程序的进程ID
    ADMIN_EMAIL = root@localhost #管理员邮件地址,它会给管理员发邮件
    SMTP_HOST = localhost
    SMTP_PORT = 25
    SMTP_FROM = DenyHosts <nobody@localhost>
    SMTP_SUBJECT = DenyHosts Report
    AGE_RESET_VALID=5d #用户的登录失败计数会在多久以后重置为0，(h表示小时，d表示天，m表示月，w表示周，y表示年)
    AGE_RESET_ROOT=25d
    AGE_RESET_RESTRICTED=25d
    AGE_RESET_INVALID=10d
    RESET_ON_SUCCESS = yes #如果一个ip登陆成功后，失败的登陆计数是否重置为0
    DAEMON_LOG = /var/log/denyhosts #自己的日志文件
    DAEMON_SLEEP = 30s #当以后台方式运行时，每读一次日志文件的时间间隔。
    DAEMON_PURGE = 1h #当以后台方式运行时，清除机制在 HOSTS_DENY 中终止旧条目的时间间隔,这个会影响PURGE_DENY的间隔。
    查看我的 /etc/hosts.deny 文件发现里面已经有 8 条记录。

    $ sudo cat /etc/hosts.deny | wc -l
    8
