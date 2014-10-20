开启win7的无线承载网络（软AP），实现网络共享
============================================

无线承载网络（Wireless Hosted Network）是Windows 7和安装有 WLAN 服务的Windows Server 2008 R2中一项新增的WLAN特性。

通过此特性，Windows 计算机能通过一块物理无线网卡以客户端身份连接到（由物理设备提供的）硬AP，同时又能作为软AP，允许其它设备与自己连接.

为了不让有些人叫板说：“这是个伪技巧，XP，vista里也可以实现无线共享啊”，所以我特此申明，XP，vista里利用传统的临时无线网（即Ad Hoc模式）来实现的，临时无线网（即Ad Hoc模式）是一种点对点网络，类似于有线网中的“双机互联”，虽然也能实现互联网共享，但主要用于两个设备临时互联，并且有的设备（如采用Android系统的设备）并不支持连接到临时无线网。还有一个很严重的问题，由于一块无线网卡只能连接到一个无线网络，因此如果通过无线网卡连接到 Internet，就不能再使用这个无线网卡建立临时网络，共享 Internet 了。而无线承载网络的原理就不一样了。它是通过把物理网卡虚拟一块虚拟网卡，从而模拟软AP的功能。所以当你物理网卡连在真正的无线路由时，可以通过模拟的软AP（软路由）供其他电脑上网。

---

这么好的东西我可不敢私藏，下面我看看怎么操作吧~~

1. 一管理员身份运行命令提示符（开始菜单——所有程序——附件，找到命令提示符，右键单击它，选择“以管理员身份运行”，在弹出的用户控制窗口中单击“是”。）然后输入命令：

	    netsh wlan show drivers

  如图，如果“支持的网络承载”一栏为“是”，那么你的网卡使用“软AP”功能。

  ![](http://biangbiangpic.b0.upaiyun.com/blog/00e9ad89f47d08be77efbdc4cda9e9b7.png)

2. 输入如下命令，启用并设定“虚拟Wifi网卡”模式：


		netsh wlan set hostednetwork mode=allow ssid=”shadowhack” key=88888888

  其中
  
  Mode：是否启用虚拟Wifi网卡，改为disallow则为禁用，虚拟网卡即会消失。
  
  Ssid：指定无线网络的名称，最好为英文。
  
  Key：指定无线网络的密码。该密码用于对无线网进行安全的WPA2加密，能够很好的防止被蹭网。
  
  以上三个参数其实可以单独使用，例如只使用 mode=disallow 可以直接禁用虚拟Wifi网卡。
  
  ![](http://biangbiangpic.b0.upaiyun.com/blog/8821c7f40982aead4a92eba856666277.png)

3. 启用“Internet连接共享（ICS）”

  为了与其他计算机或设备共享已连接的互联网，我们需要启用“Internet连接共享”功能。打开“网络和网络共享中心”窗口——“更改适配器设置”，右键单击已连接到Internet的网络连接，选择“属性”，切换到“共享”选项卡，选中其中的复选框，并选择允许其共享Internet的网络连接在这里即我们的虚拟Wifi网卡：

  ![](http://biangbiangpic.b0.upaiyun.com/blog/36a9ba466223c5d23fff3ec426516f2b.png)

4. 开启无线网络

  继续在命令提示符中运行以下命令：
  
  netsh wlan start hostednetwork

  即可开启我们之前设置好的无线网络（相当于打开路由器的无线功能。同理，将start改为stop即可关闭该无线网）。

  ![](http://biangbiangpic.b0.upaiyun.com/blog/fde6a34a2eda38e9b9d802455d485001.png)
