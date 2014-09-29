Bug in Bash shell creates big security hole on anything with \*nix in it
========================================================================

Could allow attackers to execute code on Linux, Unix, and Mac OS X.

![](http://biangbiangpic.b0.upaiyun.com/blog/c03c737d36ad428c898a975b35525106.jpg)

Mac OS X Mavericks is also a *nix, and also vulnerable to the Bash bug.

---

A security vulnerability in the GNU Bourne Again Shell (Bash), the command-line shell used in many Linux and Unix operating systems, could leave systems running those operating systems open to exploitation by specially crafted attacks. “This issue is especially dangerous as there are many possible ways Bash can be called by an application,” a Red Hat security advisory warned.

The bug, discovered by Stephane Schazelas, is related to how Bash processes environmental variables passed by the operating system or by a program calling a Bash-based script. If Bash has been configured as the default system shell, it can be used by network–based attackers against servers and other Unix and Linux devices via Web requests, secure shell, telnet sessions, or other programs that use Bash to execute scripts.

Because of its wide distribution, the vulnerability could be as wide-ranging as the Heartbleed bug, though it may not be nearly as dangerous. The vulnerability affects versions 1.14 through 4.3 of GNU Bash. Patches have been issued by many of the major Linux distribution vendors for affected versions, including:

* Red Hat Enterprise Linux (versions 4 through 7) and the Fedora distribution
* CentOS (versions 5 through 7)
* Ubuntu 10.04 LTS, 12.04 LTS, and 14.04 LTS
* Debian

A test on Mac OS X 10.9.4 ("Mavericks") by Ars showed that it also has a vulnerable version of Bash. Apple has not yet patched Bash, though it just issued an update to "command line tools."

While Bash is often thought of just as a local shell, it is also frequently used by Apache servers to execute CGI scripts for dynamic content (through mod_cgi and mod_cgid). A crafted web request targeting a vulnerable CGI application could launch code on the server. Similar attacks are possible via OpenSSH, which could allow even restricted secure shell sessions to bypass controls and execute code on the server. And a malicious DHCP server set up on a network or running as part of an “evil” wireless access point could execute code on some Linux systems using the Dynamic Host Configuration Protocol client (dhclient) when they connect.

There are other services that run on Linux and Unix systems, such as the CUPS printing system, that are similarly dependent on Bash that could be vulnerable.

There is an easy test to determine if a Linux or Unix system is vulnerable. To check your system, from a command line, type:

	env x='() { :;}; echo vulnerable' bash -c "echo this is a test"

If the system is vulnerable, the output will be:

	vulnerable
	 this is a test

An unaffected (or patched) system will output:

	 bash: warning: x: ignoring function definition attempt
	 bash: error importing function definition for `x'
	 this is a test

The fix is an update to a patched version of the Bash shell. To be safe, administrators should do a blanket update of their versions of Bash in any case.
