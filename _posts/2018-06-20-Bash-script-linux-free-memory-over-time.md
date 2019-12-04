---
published: true
---
## Linux free memory/swap for a period of time

Lets write a tiny bash script to record free memory and free swap every 3 seconds to a file for a total of 15 seconds.


#### Formatted
{% highlight bash%}
> /tmp/1.txt;
for ((i=0; i<5; i++));
do 
  d=`date -u`
  freemem=`awk '/^Mem/ {print $4}' <(free -h)`;
  freeswap=`awk '/^Swap/ {print $4}' <(free -h)`;
  echo "FreeMem=$freemem, FreeSwap=$freeswap" >> /tmp/1.txt;
  sleep 3; 
done

{% endhighlight %}

#### One liner
{% highlight bash%}
obaida@mars:~$ > /tmp/1.txt; for ((i=0; i<5; i++)); do d=`date -u`;freemem=`awk '/^Mem/ {print $4}' <(free -h)`;freeswap=`awk '/^Swap/ {print $4}' <(free -h)`; echo "$d FreeMem=$freemem, FreeSwap=$freeswap" >> /tmp/1.txt; sleep 3; done

{% endhighlight %}


#### Result/Output
{% highlight bash%}
Wed Dec  4 12:18:25 UTC 2019 FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 12:18:28 UTC 2019 FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 12:18:31 UTC 2019 FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 12:18:34 UTC 2019 FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 12:18:37 UTC 2019 FreeMem=8.8G, FreeSwap=30G
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
