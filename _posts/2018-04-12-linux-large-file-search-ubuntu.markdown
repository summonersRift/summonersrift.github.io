---
layout: post
title:  "Linux large file search"
date:   2018-04-12
---

<p class="intro"><span class="dropcap">I</span>n this post, we will see how to list largest files or directories in linux.</p>

{% highlight bash %}
du -aBM 2>/dev/null | sort -nr | head -n 50 | more
{% endhighlight %}

1. <b>du</b> arguments:<br>
<b>-a</b> for "all" files and directories. Leave it off for just directories <br>
<b>-BM</b> to output the sizes in megabyte (M) block sizes (B)<br>
<b>2>/dev/null</b> - exclude "permission denied" error messages (thanks @Oli)

2. <b>sort</b> arguments:
<b>-n</b> for "numeric"<br>
<b>-r</b> for "reverse" (biggest to smallest)

3. <b>head</b> arguments:<br>
<b>-n 50</b> for the just top 50 results.

4. Leave off <b>more</b> if using a smaller number<br>
<b>Note</b>: Prefix with sudo to include directories that your account does not have permission to access.

Example showing <b>top 10</b> biggest files and directories in </b>/var</b> (including grand total).

{% highlight bash %}
cd /var
sudo du -aBM 2>/dev/null | sort -nr | head -n 10
7555M   .
6794M   ./lib
5902M   ./lib/mysql
3987M   ./lib/mysql/my_database_dir
1825M   ./lib/mysql/my_database_dir/a_big_table.ibd
997M    ./lib/mysql/my_database_dir/another_big_table.ibd
657M    ./log
629M    ./log/apache2
587M    ./log/apache2/ssl_access.log
273M    ./cache
{% endhighlight %}


####Original Posting
[AskUbuntu] https://askubuntu.com/questions/36111/whats-a-command-line-way-to-find-large-files-directories-to-remove-and-free-up
