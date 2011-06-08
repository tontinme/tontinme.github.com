---
layout: post
title: sudoers edit
category: notes
time: 09:29:35
---
h2. /etc/sudoers分配用户权限格式

{% highlight bash %}
USER    MACHINE=(ALL)COMMANDS
{% endhighlight %}

h3. User Aliases

可以为某一组用户同时设置权限

{% highlight bash %}
User_Alias ADMINS = jsmith, mikem
{% endhighlight %}

h3. Host Aliases

指定该规则适用的主机

{% highlight bash %}
Host_Alias      FILESERVERS = fs1, fs2
Host_Alias      MAILSERVERS = smtp, smtp2
{% endhighlight %}

h3. 第三栏的值放在括号中，指出USER能够以何种身份来执行命令

h3. Command Aliases

可以方便的赋予执行某个功能的一组命令

{% highlight bash %}
Networking
Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig, /bin/ping, /sbin/dhclient, /usr/bin/net,\
                         /sbin/iptables, /usr/bin/rfcomm, /usr/bin/wvdial, /sbin/iwconfig, /sbin/mii-tool
Installation and management of software
Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum
{% endhighlight %}

h3. 指出哪些命令在执行时不需要输入密码

{% highlight bash %}
jorge ALL=(root) NOPASSWD:/bin/kill, /usr/bin/killall
{% endhighlight %}

h2. 一些sample

{% highlight bash %}
jorge ALL=(root) NOPASSWD:/bin/kill, /usr/bin/killall
{% endhighlight %}

专职系统管理员(millert,mikef和dowdy)可以在任何主机上执行任何命令而不需要进行身份验证。

{% highlight bash %}
jorge ALL=(root) NOPASSWD:/bin/kill, /usr/bin/killall
User_Alias FULLTIMERS = millert, mikef, dowdy
FULLTIMERS ALL = NOPASSWD: ALL
{% endhighlight %}

兼职系统管理员(bostley,jwfox和crawl)可以在任何主机上运行任何命令,
但他们首先必须进行身份验证(因为这个条目没有NOPASSWD标签)。

{% highlight bash %}
jorge ALL=(root) NOPASSWD:/bin/kill, /usr/bin/killall
User_Alias PARTTIMERS = bostley, jwfox, crawl
PARTTIMERS ALL = ALL
{% endhighlight %}

兼职管理员(jalala, sonar和huge)可以在任何主机上运行 BROWSE、PROCESSES、USERS 中的命令
同时可以修改除了 root 用户之外的所有用户口令

{% highlight bash %}
jorge ALL=(root) NOPASSWD:/bin/kill, /usr/bin/killall
User_Alias PARTTIMERS2 = jalala, sonar , huge
Cmnd_Alias BROWSE = /bin/ls, /bin/cd, /bin/cat
Cmnd_Alias PROCESSES = /bin/nice, /bin/kill, /usr/bin/kill, /usr/bin/killall
Cmnd_Alias USERS = /usr/sbin/useradd [A-z]*,/usr/sbin/userdel -r [A-z]*
PARTTIMERS2 ALL=USERS,PROCESSES,BROWSE,/usr/bin/passwd [A-z]*, !/usr/bin/passwd root
{% endhighlight %}

允许sys 组的成员运行 networking, software, service 等管理命令

{% highlight bash %}
jorge ALL=(root) NOPASSWD:/bin/kill, /usr/bin/killall
Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig, /bin/ping, \
                /sbin/dhclient, /usr/bin/net, /sbin/iptables, /usr/bin/rfcomm, \
                /usr/bin/wvdial, /sbin/iwconfig, /sbin/mii-tool
Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum
Cmnd_Alias SERVICES = /sbin/service, /sbin/chkconfig
Cmnd_Alias LOCATE = /usr/sbin/updatedb
Cmnd_Alias STORAGE = /sbin/fdisk, /sbin/sfdisk, /sbin/parted, /sbin/partprobe, \
                /bin/mount, /bin/umount
Cmnd_Alias DELEGATING = /usr/sbin/visudo, /bin/chown, /bin/chmod, /bin/chgrp
Cmnd_Alias PROCESSES = /bin/nice, /bin/kill, /usr/bin/kill, /usr/bin/killall
Cmnd_Alias DRIVERS = /sbin/modprobe
%sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, \
                PROCESSES, LOCATE, DRIVERS
{% endhighlight %}

WEBMASTERS(will, wendy,和 wim)中的用户都能够在主机www上
以www的身份执行任何命令，或者使用 su www 命令

{% highlight bash %}
jorge ALL=(root) NOPASSWD:/bin/kill, /usr/bin/killall
User_Alias WEBMASTERS = will, wendy, wim
WEBMASTERS www = (www) ALL, (root) /usr/bin/su www
{% endhighlight %}

