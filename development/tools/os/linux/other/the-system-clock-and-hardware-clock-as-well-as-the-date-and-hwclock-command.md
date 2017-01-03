# 系统时钟和硬件时钟以及date和hwclock命令

### 1. “系统时间”与“硬件时间”

系统时间: 一般说来就是我们执行 date 命令看到的时间，linux系统下所有的时间调用（除了直接访问硬件时间的命令）都是使用的这个时间。

硬件时间: 主板上BIOS中的时间，由主板电池供电来维持运行，系统开机时要读取这个时间，并根据它来设定系统时间（注意：系统启动时根据硬件时间设定系统时间的过程可能存在时区换算，这要视具体的系统及相关设置而定）。

### 2. “UTC时间”与“本地时间”

UTC时间：Coordinated Universal 8 e2 i( H7 t0 ^/ ^Time 世界协调时间（又称世界标准时间、世界统一时间），在一般精度要求下，它与GMT（Greenwich Mean Time，格林威治标准时间）是一样的，其实也就是说 GMT≈UTC，但 UTC 是以原子钟校准的，更精确。

本地时间：由于处在不同的时区，本地时间一般与UTC是不同的，换算方法就是

本地时间 = UTC + 时区 或 UTC = 本地时间 - 时区

时区东为正，西为负，例如在中国，本地时间都使用北京时间，在linux上显示就是 CST（China Standard Time，中国标准时，注意美国的中部标准时Central Standard Time也缩写为CST，与这里的CST不是一回事！），时区为东八区，也就是 +8 区，所以 CST=UTC+(+8小时) 或 UTC=CST-(+8小时)。

### 3. date和hwclock命令

data命令

查看系统时间

    # date

设置系统时间

    # date --set “2012-12-17 10:19"

hwclock命令

    -r, --show         读取并打印硬件时钟（read hardware clock and print result）
    -s, --hctosys      将硬件时钟同步到系统时钟（set the system time from the hardware clock）
    -w, --systohc     将系统时钟同步到硬件时钟（set the hardware clock to the current system time）

如果使用date命令修改了系统时间，并不会自动去修改硬件时钟，因此，当系统下次重启时，系统时钟还会从硬件时钟去取，date设置的时间就无效了。因此需要hwclock命令再来同步系统时钟到硬件时钟，这样下次启动的时候内核则会读取正确的硬件时间到系统时间。
