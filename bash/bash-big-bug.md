安全公告：GNU Bash 漏洞公告 (CVE-2014-6271, CVE-2014-7169)
==========================================================

CVE ID：CVE-2014-6271，CVE-2014-7169

### 漏洞描述：

CVE-2014-6271：

攻击者可构造特殊的环境变量值，以在这些环境变量的值中包含特定的代码，当 Shell 对这些环境变量求值时，这些特定的代码将得以在系统中执行。某些服务和应用接受未经身份者提供的环境变量，因此攻击者可利用此漏洞源于在提供这些服务和应用的系统上执行任意的 Shell 命令。

CVE-2014-7169：

因 GNU Bash 允许在环境变量的值中的函数定义，及在函数定义后加入额外的字符串，攻击者可利用此特性在远程写入文件或执行其他可以影响到系统的操作。

以上两漏洞可能会影响到使用 ForceCommand 功能的 OpenSSH sshd； 使用 mod_cgi 或 mod_cgid 的 Apache 服务器； 调用 Shell 配置系统的 DHCP 客户端； 及其他使用 bash 作为解释器的应用等。

注：

1） CVE-2014-7169 的存在是因 CVE-2014-6271 的 Patch 不完整。

2） 关于以上两漏洞的更多详情请见 [1], [2], [3]

### 问题诊断：

如何确定当前 bash 版本是否有漏洞

	$ env x='() { :;}; echo vulnerable' bash -c "echo this is a test"

若输出以下内容

	vulnerable
	this is a test

则证明系统当前的 Bash 版本存在漏洞。

若输出如下

	$ env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
	bash: warning: x: ignoring function definition attempt
	bash: error importing function definition for `x'
	this is a test

则证明当前的 Bash 已为漏洞修复版本

### 各系统修补方式：

	* CentOS/Fedora/RHEL
	# yum install bash

	* Debian/Ubuntu
	# apt-get update; apt-get install bash

	* OpenSUSE
	# zypper refresh; zypper update bash

	* Archlinux
	# pacman -Sy bash

若所用发行版本的软件仓库中还没有包含漏洞修复的 Bash 版本，请下载相应版本的源码包及相应的 patch 后，自行编译安装。相应的 patch 可通过以下链接获得：

http://ftp.gnu.org/gnu/bash/bash-${version}-patches/

例如，假设你的系统当前的 Bash 版本为 3.0。则相应的 patch 可通过以下链接获得：

http://ftp.gnu.org/gnu/bash/bash-3.0-patches/

Reference:

[1] https://access.redhat.com/articles/1200223

[2] https://securityblog.redhat.com/2014/09/24/bash-specially-crafted-environment-variables-code-injection-attack/

[3] https://access.redhat.com/security/cve/CVE-2014-7169
