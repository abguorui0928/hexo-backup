title: memcached tutorials
date: 2015-09-08 10:12:10
tags:
---
### 1. 安装libevent

> <font size=2>\# wget https://sourceforge.net/projects/levent/files/libevent/libevent-2.0/libevent-2.0.22-stable.tar.gz
> \# tar -zxvf libevent-2.0.22-stable.tar.gz
> \# cd libevent-2.0.22-stable
> \# ./configure --prefix=/usr
> \# make
> \# make install

备注：可选参数--prefix=/usr用于指定libevent库安装位置，默认值是/usr/local，如果保持默认值，会将库安装到/usr/local/lib目录中

### 2. 检查libevent是否安装成功
> <font size=2>\# ls -al /usr/local/lib | grep libevent

![](/images/libevent_state.png)
如上图所示，libevent安装成功，下面来安装memcached

### 3. 安装memcached
> <font size=2>\# wget http://www.memcached.org/files/memcached-1.4.24.tar.gz
> \# tar -zxvf memcached-1.4.24.tar.gz
> \# cd memcached-1.4.24
> \# ./configure
> \# make
> \# make install

### 4. 检查memcached是否安装成功
> <font size=2>\# ls -al /usr/local/bin/memcached

![](/images/memcached_state.png)
如上图所示，至此memcached安装完成

### 5. 启动脚本
	#!/bin/sh
	#
	# memcached:    MemCached Daemon
	# chkconfig:    - 90 25
	# description:  MemCached Daemon
	#
	# Source function library.
	. /etc/rc.d/init.d/functions
	. /etc/sysconfig/network
	
	start()
	{
        echo -n $"Starting memcached: "
        daemon /usr/local/bin/memcached -u root -d -m 4096 -p 11211
        echo
	}
	
	stop()
	{
        echo -n $"Shutting down memcached: "
        killproc memcached
        echo
	}
	
	[ -f /usr/local/bin/memcached ] || exit 0
	
	# See how we were called.
	case "$1" in
	start)
		start
        ;;
	stop)
        stop
        ;;
	restart|reload)
        stop
        start
        ;;
	condrestart)
        stop
        start
        ;;
	*)
        echo $"Usage: $0 {start|stop|restart|reload|condrestart}"
        exit 1
	esac
	exit 0

### 6. 示例代码
https://github.com/abguorui0928/tutorials.git