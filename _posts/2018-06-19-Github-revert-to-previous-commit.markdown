---
layout: post
title:  "Git revert to previous commit"
date:   2018-06-19
---

<p class="intro"><span class="dropcap">G</span>it revert to previous commit</p>
After several commits to your git repository, if you would like to go back to a previous stable commit--

<b>1. Check the log to find the desired commit</b><br>
git log<br><br>
<b>2. Checkout desired commit (hash) -- dont forget .</b><br>
git checkout 39808f99f0b44654717e9f1a46814816111cb .<br><br>
<b>3/4. Make changes if you want, and commit, then push</b><br>
git commit -m "particles removed"<br>
git push<br>

<b>Terminal— </b>
{% highlight bash %}
#Summary
git log
git checkout 39808f99f0b44654717e9f1a46814816111cb .
git commit -m "particles removed"
git push 

#terminal output
user@ubuntu ijcode.github.io:git log
commit a8bc92f7515c404b5e5bba52210fd1b33dc43
Author: Mo... ... user <user---@gmail.com>
Date:   Fri Jun 15 05:00:20 2018 -0400

    particles fix 6

commit 39808f99f0b44654717e9f1a46814816111cb
Author: Mo... ... user <user---@gmail.com>
Date:   Fri Jun 15 04:46:07 2018 -0400

    particles fix 5

.....................
.....................
user@ubuntu ijcode.github.io:git checkout 39808f99f0b44654717e9f1a46814816111cb .
(dont forget the .)

user@castor ijcode.github.io:git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	modified:   index.html
#
user@ubuntu ijcode.github.io:git commit -m "particles removed"
[master 75b8f73] particles removed
 1 file changed, 1 insertion(+), 13 deletions(-)
user@castor ijcode.github.io:git push
To https://github.com/ijcode/ijcode.github.io
   a8bc92f..75b8f73  master -> master

{% endhighlight %}

