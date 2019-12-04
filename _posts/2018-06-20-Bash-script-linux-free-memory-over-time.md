---
published: true
---
## Linux free memory/swap for a period of time

Lets write a tiny bash script to record free memory and free swap every 3 seconds to a file for a total of 15 seconds.

{{ "{% highlight bash" }}%} <br><br>
obaida@mars:~/thesis/aedem/code$> /tmp/1.txt; for ((i=0; i<=5; i++)); do date -u>>/tmp/1.txt;freemem=`awk '/^Mem/ {print $4}' <(free -h)`;freeswap=`awk '/^Swap/ {print $4}' <(free -h)`; echo "FreeMem=$freemem, FreeSwap=$freeswap" >> /tmp/1.txt; sleep 3; done

{{ "{% endhighlight" }}%}

#### Result/Output
{{ "{% highlight bash" }}%} <br><br>
obaida@mars:~/thesis/aedem/code$ cat /tmp/1.txt
Wed Dec  4 11:56:25 UTC 2019
FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 11:56:28 UTC 2019
FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 11:56:31 UTC 2019
FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 11:56:34 UTC 2019
FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 11:56:37 UTC 2019
FreeMem=8.8G, FreeSwap=30G
Wed Dec  4 11:56:40 UTC 2019
FreeMem=8.8G, FreeSwap=30G

obaida@mars:~/thesis/aedem/code$ free -h
              total        used        free      shared  buff/cache   available
Mem:            15G        1.8G        8.8G        179M        4.9G         13G
Swap:           30G          0B         30G
{{ "{% endhighlight" }}%}


obaida@mars:~/thesis/aedem/code$ free -m
              total        used        free      shared  buff/cache   available
Mem:          15989        1873        9049         179        5066       13608
Swap:         31470           0       31470
