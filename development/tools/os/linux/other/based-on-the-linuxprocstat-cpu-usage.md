# Linux根据/proc/stat计算cpu的使用率 

### 1、/proc/stat文件的介绍

在Linux下，CPU利用率分为用户态，系统态和空闲态，分别表示CPU处于用户态执行的时间，系统内核执行的时间，和空闲系统进程执行的时间，三者之和就是CPU的总时间，当没有用户进程、系统进程等需要执行的时候，CPU就执行系统缺省的空闲进程。从平常的思维方式理解的话，CPU的利用率就是非空闲进程占用时间的比例，即CPU执行非空闲进程的时间 / CPU总的执行时间。

在Linux系统中，CPU时间的分配信息保存在/proc/stat文件中，利用率的计算应该从这个文件中获取数据。文件的头几行记录了每个CPU的用户态，系统态，空闲态等状态下分配的时间片（单位是Jiffies），这些数据是从CPU加电到当前的累计值。常用的监控软件就是利用/proc /stat里面的这些数据来计算CPU的利用率的。

不同版本的linux /proc/stat文件内容不一样，以Linux 2.6来说，/proc/stat文件的内容如下：

    cpu 2032004 102648 238344 167130733 758440 15159 17878 0
    cpu0 1022597 63462 141826 83528451 366530 9362 15386 0
    cpu1 1009407 39185 96518 83602282 391909 5796 2492 0
    intr 303194010 212852371 3 0 0 11 0 0 2 1 1 0 0 3 0 11097365 0 72615114 
    6628960 0 179 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 
    0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 
    0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 
    0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 
    0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 
    0 0 0 0 0 0 0 0 0 0 0
    ctxt 236095529
    btime 1195210746
    processes 401389
    procs_running 1
    procs_blocked 0

第一行的数值表示的是CPU总的使用情况，所以我们只要用第一行的数字计算就可以了。下表解析第一行各数值的含义：

参数

解析（单位：jiffies）
 
user (2032004)

从系统启动开始累计到当前时刻，用户态的CPU时间，不包含 nice值为负进程。
 
nice (102648)

从系统启动开始累计到当前时刻，nice值为负的进程所占用的CPU时间
 
system (238344)

从系统启动开始累计到当前时刻，核心时间
 
idle (167130733)

从系统启动开始累计到当前时刻，除IO等待时间以外其它等待时间
 
iowait (758440)

从系统启动开始累计到当前时刻，IO等待时间
 
irq (15159)

从系统启动开始累计到当前时刻，硬中断时间
 
softirq (17878)

从系统启动开始累计到当前时刻，软中断时间

“intr”这行给出中断的信息，第一个为自系统启动以来，发生的所有的中断的次数；然后每个数对应一个特定的中断自系统启动以来所发生的次数。

“ctxt”给出了自系统启动以来CPU发生的上下文交换的次数。

“btime”给出了从系统启动到现在为止的时间，单位为秒。

“processes (total_forks) 自系统启动以来所创建的任务的个数目。

“procs_running”：当前运行队列的任务的数目。

“procs_blocked”：当前被阻塞的任务的数目。


##### 思路一

CPU时间=user+system+nice+idle+iowait+irq+softirq

那么CPU利用率可以使用以下两个方法。先取两个采样点，然后计算其差值：

cpu usage=(idle2-idle1)/(cpu2-cpu1)*100

cpu usage=[(user_2 +sys_2+nice_2) - (user_1 + sys_1+nice_1)]/(total_2 - total_1)*100

##### 思路二

因为/proc/stat中的数值都是从系统启动开始累计到当前时刻的积累值，所以需要在不同时间点t1和t2取值进行比较运算，当两个时间点的间隔较短时，就可以把这个计算结果看作是CPU的即时利用率。

CPU的即时利用率的计算公式：

CPU在t1到t2时间段总的使用时间 = ( user2+ nice2+ system2+ idle2+ iowait2+ irq2+ softirq2) - ( user1+ nice1+ system1+ idle1+ iowait1+ irq1+ softirq1)

CPU在t1到t2时间段空闲使用时间 = (idle2 - idle1)

CPU在t1到t2时间段即时利用率 =  1 - CPU空闲使用时间 / CPU总的使用时间

### 2、shell代码示例

    #!/bin/bash
    #cpu使用率
    interval=3
    cpu_num=`cat /proc/stat | grep cpu[0-9] -c`
    start_idle=()
    start_total=()
    cpu_rate=()
    cpu_rate_file=./`hostname`_cpu_rate.csv
    if [ -f ${cpu_rate_file} ]; then
        mv ${cpu_rate_file} ${cpu_rate_file}.`date +%m_%d-%H_%M_%S`.bak
    fi
    for((i=0;i<${cpu_num};i++))
    {
        echo -n "cpu$i," >> ${cpu_rate_file}
    }
    echo -n "cpu" >> ${cpu_rate_file}
    echo "" >> ${cpu_rate_file}
    while(true)
    do
        for((i=0;i<${cpu_num};i++))
        {
            start=$(cat /proc/stat | grep "cpu$i" | awk '{print $2" "$3" "$4" "$5" "$6" "$7" "$8}')
            start_idle[$i]=$(echo ${start} | awk '{print $4}')
            start_total[$i]=$(echo ${start} | awk '{printf "%.f",$1+$2+$3+$4+$5+$6+$7}')
        }
        start=$(cat /proc/stat | grep "cpu " | awk '{print $2" "$3" "$4" "$5" "$6" "$7" "$8}')
        start_idle[${cpu_num}]=$(echo ${start} | awk '{print $4}')
        start_total[${cpu_num}]=$(echo ${start} | awk '{printf "%.f",$1+$2+$3+$4+$5+$6+$7}')
        sleep ${interval}
        for((i=0;i<${cpu_num};i++))
        {
            end=$(cat /proc/stat | grep "cpu$i" | awk '{print $2" "$3" "$4" "$5" "$6" "$7" "$8}')
            end_idle=$(echo ${end} | awk '{print $4}')
            end_total=$(echo ${end} | awk '{printf "%.f",$1+$2+$3+$4+$5+$6+$7}')
            idle=`expr ${end_idle} - ${start_idle[$i]}`
            total=`expr ${end_total} - ${start_total[$i]}`
            idle_normal=`expr ${idle} \* 100`
            cpu_usage=`expr ${idle_normal} / ${total}`
            cpu_rate[$i]=`expr 100 - ${cpu_usage}`
            echo "The CPU$i Rate : ${cpu_rate[$i]}%"
            echo -n "${cpu_rate[$i]}," >> ${cpu_rate_file}
        }
        end=$(cat /proc/stat | grep "cpu " | awk '{print $2" "$3" "$4" "$5" "$6" "$7" "$8}')
        end_idle=$(echo ${end} | awk '{print $4}')
        end_total=$(echo ${end} | awk '{printf "%.f",$1+$2+$3+$4+$5+$6+$7}')
        idle=`expr ${end_idle} - ${start_idle[$i]}`
        total=`expr ${end_total} - ${start_total[$i]}`
        idle_normal=`expr ${idle} \* 100`
        cpu_usage=`expr ${idle_normal} / ${total}`
        cpu_rate[${cpu_num}]=`expr 100 - ${cpu_usage}`
        echo "The average CPU Rate : ${cpu_rate[${cpu_num}]}%"
        echo -n "${cpu_rate[${cpu_num}]}" >> ${cpu_rate_file}
        echo "------------------"
        echo "" >> ${cpu_rate_file}
    done
