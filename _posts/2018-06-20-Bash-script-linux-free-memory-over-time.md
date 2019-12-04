---
published: true
---
## Linux free memory/swap for a period of time and Top memory consumer

Lets write a tiny bash script to record free memory and free swap every 1 seconds to a file for a total of 5 seconds.


#### Basc Script
{% highlight bash%}
> /tmp/1.txt;
for ((i=0; i<5; i++));
do 
  d=`date -u`
  freemem=`awk '/^Mem/ {print $4}' <(free -h)`;
  freeswap=`awk '/^Swap/ {print $4}' <(free -h)`;
  offender=`ps aux --sort=-%mem | awk 'NR<3 {print $0}'|awk '/ / {print$4" "$11}'|awk 'FNR>1 {print $0}'`
  echo "$d FreeMem=$freemem, FreeSwap=$freeswap; TopOffender(%mem proc)=($offender)" >> /tmp/1.txt;
  sleep 3; 
done

{% endhighlight %}

#### One liner
{% highlight bash%}
> /tmp/1.txt; for ((i=0; i<5; i++)); do d=`date -u`;freemem=`awk '/^Mem/ {print $4}' <(free -h)`;freeswap=`awk '/^Swap/ {print $4}' <(free -h)`; offender=`ps aux --sort=-%mem | awk 'NR<3 {print $0}'|awk '/ / {print$4" "$11}'|awk 'FNR>1 {print $0}'`; echo "$d FreeMem=$freemem, FreeSwap=$freeswap; TopOffender(%mem proc)=($offender)" >> /tmp/1.txt; sleep 1; done

{% endhighlight %}


#### Result/Output
{% highlight bash%}
Wed Dec  4 12:45:18 UTC 2019 FreeMem=8.8G, FreeSwap=30G; TopOffender(%mem proc)=(2.7 /usr/lib/firefox/firefox)
Wed Dec  4 12:45:19 UTC 2019 FreeMem=8.8G, FreeSwap=30G; TopOffender(%mem proc)=(2.7 /usr/lib/firefox/firefox)
Wed Dec  4 12:45:20 UTC 2019 FreeMem=8.8G, FreeSwap=30G; TopOffender(%mem proc)=(2.7 /usr/lib/firefox/firefox)
Wed Dec  4 12:45:21 UTC 2019 FreeMem=8.8G, FreeSwap=30G; TopOffender(%mem proc)=(2.7 /usr/lib/firefox/firefox)
Wed Dec  4 12:45:22 UTC 2019 FreeMem=8.8G, FreeSwap=30G; TopOffender(%mem proc)=(2.7 /usr/lib/firefox/firefox)
{% endhighlight %}

#### Output of sample bash commands
{% highlight bash%}
obaida@mars:~$ free -h
              total        used        free      shared  buff/cache   available
Mem:            15G        1.8G        8.8G        179M        4.9G         13G
Swap:           30G          0B         30G

obaida@mars:~$ free -m
              total        used        free      shared  buff/cache   available
Mem:          15989        1873        9049         179        5066       13608
Swap:         31470           0       31470
{% endhighlight %}


#### Top offender/ memory consumer process name
{% highlight bash%}
obaida@ganymede:~$ ps aux --sort=-%mem | awk 'NR<3 {print $0}'|awk '/ / {print$4" "$11}'|awk 'FNR>1 {print $0}'
2.7 /usr/lib/firefox/firefox

obaida@ganymede:~$ ps aux --sort=-%mem | awk 'NR<10 {print $0}'|awk '/ / {print$4" "$11}'
%MEM COMMAND
2.7 /usr/lib/firefox/firefox
2.1 /usr/lib/firefox/firefox
1.4 /usr/lib/firefox/firefox
1.3 /usr/lib/firefox/firefox
1.2 /usr/lib/firefox/firefox
1.1 /usr/lib/xorg/Xorg
1.0 /usr/bin/gnome-shell
0.9 /usr/bin/python3
{% endhighlight %}