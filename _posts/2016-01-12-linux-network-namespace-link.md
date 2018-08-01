---
published: true
---

Linux network namespaces 
In this post we will create two linux network namespaces that can handle packets and establish communication among themselves(t1 and t2) using the kernel **ip_forward**.


The network looks like the following--

  **t1** ---- link1 ---- **kernel** ---- link2 ----**t2**
  

Clean up the interfaces from any previous experiments--
{% highlight bash %}
sudo ip netns del t1
sudo ip link del t1-eth0
sudo ip link del eth-t1
sudo ip netns del t2
sudo ip link del t2-eth0
sudo ip link del eth-t2
{% endhighlight %}


Create new link with hwaddr, ip addresss, network namespaces.
{% highlight bash %}
#creating namespaces t1 and t2
sudo ip netns add t1
sudo ip netns add t2

#creating link1 and link2
sudo ip link add t1-eth0 type veth peer name eth-t1
sleep 1
sudo ip link add t2-eth0 type veth peer name eth-t2
sleep 1

#assign the newly created interfaces to the hosts/namespaces
sudo ip link set t1-eth0 netns t1
sudo ip link set t2-eth0 netns t2

#configure ip address
sudo ip netns exec t1 ifconfig t1-eth0 10.10.251.1/24
sudo ip netns exec t2 ifconfig t2-eth0 10.10.252.1/24

#start loopback addresses
sudo ip netns exec t1 ifconfig lo up
sudo ip netns exec t2 ifconfig lo up

#assign HWADDR to the interfaces
ifconfig eth-t1 hw ether 02:01:02:03:04:01
ifconfig eth-t2 hw ether 02:01:02:03:04:02

#assign ip and activate the ethernet interfaces
ifconfig eth-t1 10.10.251.2/24 up
ifconfig eth-t2 10.10.252.2/24 up
{% endhighlight %}

Test the namespaces with ping and iperf traffic-
{% highlight bash %}
#test with ping
ping 10.10.251.1
ping 10.10.252.1
sudo ip netns exec t1 ping 10.10.251.2
sudo ip netns exec t2 ping 10.10.252.2

#iperf, traffic test with server and client
sudo ip netns t1 iperf -s
sudo ip netns t2 iperf -c 10.10.251.1

#ping
sudo ip netns t1 ping 10.10.252.1
{% endhighlight %}
If there is no forwarding mechanism available to forward packets from 10.10.251.0/24 and 10.10.252.0/24 network we would need the ip_forward enabled in the kernel

{% highlight bash %}
cat /proc/sys/net/ipv4/ip_forward
0
{% endhighlight %}

Change the **ip_forward** flag value
{% highlight bash %}
echo 1 > /proc/sys/net/ipv4/ip_forward
{% endhighlight %}

Altenatively, 
{% highlight bash %}
sysctl -w net.ipv4.ip_forward=1
{% endhighlight %}
