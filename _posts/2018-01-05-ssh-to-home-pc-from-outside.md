---
published: true
---

<p class="intro"><span class="dropcap">I</span>n this post I will discuss how I setup ssh to home pc so that I can access it from anywhere else (office, cafe, blah, blah!!)</p>


### Overview

| Step | Description |
| --- | --- |
| Step1  | **Setup SSH on Desktop**: Install openssh-server and start it up, so that you can ssh to it locally from another computer in the same network|
| Step2  | **PUBLIC_IP**: From any computer in your home network, find out what is your public ip by goign to whatismyip.com |
| Step3 | **Port forwarding**: All traffic destined to port 22(default for ssh) should go to DESKTOP_IP:22 (an example figure given below)|
| Step4 | SSH from anywhere to PUBLIC_IP which will take you to DESKTOP_IP or your desktop|


#### Step 1: Setup Desktop

{% highlight bash %}
#install openssh-server on linux
sudo apt update
sudo apt upgrade
sudo apt install openssh-server


#Test if ssh is running by following systemctl command:
sudo systemctl status ssh

#If not running, use the following to start ssh
sudo systemctl enable ssh
sudo systemctl start ssh
{% endhighlight %}

#### Step 2: Getting Ip address from command line

<pre>

dig TXT +short o-o.myaddr.l.google.com @ns1.google.com | awk -F'"' '{ print $2}'

</pre>

#### Step 3: Port forwarding
Usually you can do it by going to 192.168.0.1 or the admin page of your router/modem.

<figure>
	<img src="{{ '/assets/img/ssh-setup-tp-link.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>SSH forwarding on TP-link router (FYI: My building has wiring built in, doesnt need modem) </figcaption>
</figure>

#### Step4: SSH to PUBLIC IP you got from step-2

{% highlight bash %}
ssh user@PUBLIC_IP
{% endhighlight %}

