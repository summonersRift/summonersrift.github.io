---
published: true
---

Upload files from local directory to a remote hosting server using ftp

<figure>
    <img src="/assets/img/BlueHost-FTP-Access.jpg" alt=""> 
    <figcaption>FTP (c) BlueHost</figcaption>
</figure>


#### Ftp to server IP
{% highlight bash%}
userX@local-pc tmp:ftp 204.100.100.100
Connected to 204.100.100.100.

XXX User userX OK. Password required
Password:
XXX OK. Current restricted directory is /
Remote system type is UNIX.
Using binary mode to transfer files.
{% endhighlight %}


#### Transfer the file
use  __put local-file remote file__ command to transfer 
{% highlight bash%}
ftp> put /tmp/test.txt /public_html/test.txt
local: /tmp/test.txt remote: /public_html/test.txt
XXX PORT command successful
XXX Connecting to port 43271
XXX-File successfully transferred
XXX 0.058 seconds (measured here), 0.69 Kbytes per second
41 bytes sent in 0.00 secs (1.8619 MB/s)

{% endhighlight %}


#### Reference
* https://superuser.com/questions/466573/how-do-i-copy-a-file-over-ftp-using-ubuntu-linux

