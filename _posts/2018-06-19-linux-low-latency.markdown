---
layout: post
title:  "Git revert to previous commit"
date:   2018-06-19
---

<p class="intro"><span class="dropcap">L</span>inux <b>low-latency</b> related knowledge</p>
An attempt to gather resources on low-latency linux. <i>Musicians</i> use low-latency linux for recording and live performance. In stock market ticker data streaming one will need low-latency on per packet processing. <br>
A resource on preparing <a href="https://help.ubuntu.com/community/UbuntuStudioPreparation">UbuntuStudio</a> specially for musicians.

First things first, if you need lower latency, get away from ubuntu and use debian or a small footprint OS image + kernel.

<b>Zero-copy</b>
Zero copy lets you avoid redundant data copies between intermediate buffers and reduces the number of context switches between user-space and kernel-space. Ideal Zero copy (zero cpu copy) is possible when your hardware (disk drive, network card, graphic card, sound card) supports DMA (Direct Memory Acess).

<b>File system zero-copy</b>: reduce user-space, kernel-space interaction for file copying
Check this code in github <a href ="https://github.com/arturmkrtchyan/zero-copy">Filesystem zero-copy in C/Java</a>.


<b>Q:</b> How to install the low-latency kernel in Ubuntu 16.04? following might work<br>
<code>sudo apt-get install linux-image-lowlatency</code>


<b>Playing with sysctl flags (offered by kernel to configure)</b><br>
Here are some of the sysctl flags  to adjust kernel behavior.<br>





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



<b>Terminal— </b>
{% highlight bash %}
code sample
{% endhighlight %}

<h4>Good references</h4>
* https://blog.cloudflare.com/how-to-achieve-low-latency/
