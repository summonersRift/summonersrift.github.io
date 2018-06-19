---
layout: post
title:  "Linux low-latency"
date:   2018-06-19
---

<p class="intro"><span class="dropcap">L</span>inux <b>low-latency</b> related knowledge</p>
An attempt to gather resources on low-latency linux. <i>Musicians</i> use low-latency linux for recording and live performance. In stock market ticker data streaming one will need low-latency on per packet processing. <br>
A resource on preparing <a href="https://help.ubuntu.com/community/UbuntuStudioPreparation">UbuntuStudio</a> specially for musicians.

First things first, if you need lower latency, get away from ubuntu and use Debian or Redhat (small footprint) OS image + kernel.

<b>Zero-copy</b>
Zero copy lets you avoid redundant data copies between intermediate buffers and reduces the number of context switches between user-space and kernel-space. Ideal Zero copy (zero cpu copy) is possible when your hardware (disk drive, network card, graphic card, sound card) supports DMA (Direct Memory Acess).

<b>File system zero-copy</b>: reduce user-space, kernel-space interaction for file copying
Check this code in github <a href ="https://github.com/arturmkrtchyan/zero-copy">Filesystem zero-copy in C/Java</a>.


<b>Q:</b> How to install the low-latency kernel in Ubuntu 16.04? following might work<br>
<code>sudo apt-get install linux-image-lowlatency</code>


Playing with <b>sysctl</b> flags (offered by kernel to configure)</b><br>
Two servers located in two different data center. Both server deals with a lot of concurrent large file transfersa. But network performance is very poor for large files and performance degradation take place with a large files. How do I tune TCP under Linux to solve this problem? <a href="https://www.cyberciti.biz/files/linux-kernel/Documentation/networking/ip-sysctl.txt">Documentation</a>
Here are some of the sysctl flags  to adjust kernel behavior.<br>
{% highlight bash %}
--Default maximum Linux TCP buffer size
cat /proc/sys/net/ipv4/tcp_mem
--The default and maximum amount for the receive socket memory
cat /proc/sys/net/core/rmem_default
cat /proc/sys/net/core/rmem_max
--The default and maximum amount for the send socket memory:
cat /proc/sys/net/core/wmem_default
cat /proc/sys/net/core/wmem_max
--The maximum amount of option memory buffers:
cat /proc/sys/net/core/optmem_max


--Tune values
echo 'net.core.wmem_max=12582912' >> /etc/sysctl.conf
echo 'net.core.rmem_max=12582912' >> /etc/sysctl.conf
--You also need to set minimum size, initial size, and maximum size in bytes:
echo 'net.ipv4.tcp_rmem= 10240 87380 12582912' >> /etc/sysctl.conf
echo 'net.ipv4.tcp_wmem= 10240 87380 12582912' >> /etc/sysctl.conf
--Turn on window scaling which can be an option to enlarge the transfer window:
echo 'net.ipv4.tcp_window_scaling = 1' >> /etc/sysctl.conf
--Enable timestamps as defined in RFC1323:
echo 'net.ipv4.tcp_timestamps = 1' >> /etc/sysctl.conf
--Enable select acknowledgments:
echo 'net.ipv4.tcp_sack = 1' >> /etc/sysctl.conf
--By default, TCP saves various connection metrics in the route cache when the connection closes, so that connections established in the near future can use these to set initial conditions. Usually, this increases overall performance, but may sometimes cause performance degradation. If set, TCP will not cache metrics on closing connections.
echo 'net.ipv4.tcp_no_metrics_save = 1' >> /etc/sysctl.conf

--Set maximum number of packets, queued on the INPUT side, when the interface receives packets faster than kernel can process them.
echo 'net.core.netdev_max_backlog = 5000' >> /etc/sysctl.conf

--Now reload the changes:
sysctl -p

--Use tcpdump to view changes for eth0:
tcpdump -ni eth0

--done_
{% endhighlight %}



These are some simple <b>guidelines on selecting the right kernel</b> that fits your need (in a particularorder)--by ubuntu fan <a href="https://askubuntu.com/questions/126664/why-choose-a-low-latency-kernel-over-a-generic-or-realtime-one">askubuntu</a>
<ul>
  <li>If you do not require low latency for your system then please use the -generic kernel.</li>
  <li>If you need a low latency system (e.g. for recording audio) then please use the -preempt kernel as a first choice. This reduces latency but doesnt sacrifice power saving features. It is available only for 64 bit systems (amd64).</li>
  <li>If the -preempt kernel does not provide enough low latency for your needs (or you have an 32 bit system) then you should try the -lowlatency kernel.</li>
  <li>If the -lowlatency kernel isn't enough then you should try the -rt kernel</li>
  <li>If the -rt kernel isn't enough stable for you then you should try the -realtime kernel</li>
</ul>

The most relevant kernel options if you want to recompile your kernel yourself to have a low-latency desktop:<br>
<code>PREEMPT=y</code>
and:
<code>CONFIG_1000_HZ=y</code>
To add some powersaving check this one:
<code>CONFIG_NO_HZ=y</code>

Additionally, preemt, lowlatency or the rt kernel wont make your system faster (for general tasks?). They are slightly slower than generic kernel.


<h4>Good references</h4>
* https://blog.cloudflare.com/how-to-achieve-low-latency/
* redhat tuning: <a href="https://access.redhat.com/sites/default/files/attachments/201501-perf-brief-low-latency-tuning-rhel7-v1.1.pdf">Redhat low-latency config guide </a>
* Linux tcp tuning: https://www.cyberciti.biz/faq/linux-tcp-tuning/
