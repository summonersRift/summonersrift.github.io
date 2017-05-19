---
layout: post
title:  "LXC Ubuntu 12.04 Creating containers with veth and Setting IP hwaddr"
date:   2014-11-19
---


<p class="intro"><span class="dropcap">L</span>XC containers in ubuntu</p>

<p class="western" align="CENTER"><span style="font-size: large;"><b>Creating LXC containers in ubuntu 12.04 LTS and setting IP, veth</b></span></p>
<p class="western" style="text-align: left;" align="CENTER">Say you dont want to use the default lxcbr0 created by the lxc and want to have your own linux bridge.</p>

<ul>
   <li><strong>Create two containers container 1 and container 2</strong></li>
</ul>
<pre>sudo lxc-create -t ubuntu -n container1
sudo lxc-create -t ubuntu -n container2</pre>
<p class="western">Creating first container always takes longer time. You can use lxc-clone as well for faster creation of the containers</p>

<ul>
   <li class="western"><strong>Deleting the default bridge that lxc will create and make containers without bridge and a variable veth.</strong></li>
</ul>
<pre>sudo vi /var/lib/lxc/container1/config</pre>
<p class="western"><em>Remove</em>  the line with default bridge(delete following line from the config file):</p>

<pre>lxc.network.link = lxcbr0</pre>
<p class="western">Change hwaddr with whatever u want and add the veth pair to be whatever you want (say i want veth 55 connected to container1 and have hwaddr 00:00:00:00:00:37). Add veth.pair if its not already there.</p>

<pre>lxc.network.hwaddr = 00:00:00:00:00:37
lxc.network.veth.pair = veth55</pre>
<p class="western">The following command may or may not work if you insert it to config file</p>

<pre>lxc.network.ipv4 = 192.168.0.9/24</pre>
<p class="western">Do the same for container 2.</p>

<pre>sudo vi /var/lib/lxc/container2/config</pre>
<p class="western">Delete following:</p>

<pre>lxc.network.link = lxcbr0</pre>
<p class="western">Insert following:</p>

<pre>lxc.network.hwaddr = 00:00:00:00:00:d8
lxc.network.veth.pair = veth216</pre>
<p class="western">The following command may or may not work if you insert it to config file</p>

<pre>lxc.network.ipv4 = 192.168.0.41/24</pre>
<ul>
   <li class="western"><strong>Starting containers in background mode. (-d)</strong></li>
</ul>
<pre>sudo lxc-start -d -n container1
sudo lxc-start -d -n container2</pre>
<ul>
   <li class="western"><strong>Check to if the containers are running and debug</strong></li>
</ul>
<pre>sudo lxc-list --fancy</pre>
<p class="western" align="LEFT"><strong>[Optional-if the containers aren't running or stopped]</strong> If the containers are not running however stopped which means something went wrong. You can always check what went wrong by trying to start container as following</p>

<pre>sudo lxc-start -n container1</pre>
<p class="western" align="LEFT">Now search internet for whatever problem you might have.</p>
<p class="western" align="LEFT">Lets get back to business.</p>

<ul>
   <li class="western"><strong>Entering the containers to set IP</strong></li>
</ul>
<pre>sudo lxc-attach -n container1
ifconfig eth0 192.168.0.9 netmask 255.255.255.0 up</pre>
<pre>sudo lxc-attach -n container2
ifconfig eth0 192.168.0.41 netmask 255.255.255.0 up</pre>
<p class="western" align="LEFT">Exit from the containers with just an <strong>exit </strong>command.</p>

<ul>
   <li class="western"><b>In HOST machine we have to deletede fault bridge (if there is any) and create net veth</b></li>
   <li class="western">Show all the bridges
<pre>brctl show</pre>
</li>
   <li class="western">Deleting default (any) bridge
<pre>ip link set lxcbr0 down
brctl delbr lxcbr0</pre>
</li>
   <li class="western">Check to see if the bridge is still there
<pre>ifconfig -a</pre>
</li>
   <li class="western">Add your own linux bridge <em>skbbr</em>
<pre>brctl addbr skbbr</pre>
</li>
</ul>
<ul>
   <li class="western">Add interfaces to bridge:
<pre>brctl addif skbbr veth55
brctl addif skbbr veth216</pre>
</li>
   <li class="western">Show all the bridges. this time you will see the interfaces <strong>veth55</strong> and <strong>veth216</strong> added to the bridge <strong>skbbr</strong>
<pre>brctl show</pre>
</li>
   <li class="western">Check <strong>ifconfig</strong> to see if the bridge is up. Look for <strong>skbbr</strong>.  If you don't see it but you can see it in <strong>ifconfig -a </strong>then you need to bring the bridge up.
<pre>ip link set skbbr up</pre>
</li>
   <li class="western"><b>Now you can log into any container and ping from one to another.</b>
<pre>sudo lxc-attach -n container1
ping 192.168.0.41</pre>
</li>
</ul>
Don't ask questions in this blog please, I don't have time to answer those.

Thanks

obaida
