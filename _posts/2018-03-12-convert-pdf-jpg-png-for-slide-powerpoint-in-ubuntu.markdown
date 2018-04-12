---
layout: post
title:  "Convert PDF to PNG JPG in Ubuntu"
date:   2017-03-12
---

<p class="intro"><span class="dropcap">I</span>n this post, we will see how to covert PDF files to high quality JPG PNG or any other formats in Ubuntu using ImageMagick.</p>

This is a short tips copied from <a href="https://askubuntu.com/questions/50170/how-to-convert-pdf-to-image/50180">askubuntu</a> answered by BinaryLife/Tim.

#### Install
First, install <a href="http://apt.ubuntu.com/p/imagemagick">imagemagick</a>
{% highlight bash %}
sudo apt-get install imagemagick{% endhighlight  %}

#### Convert
Convert the pdf using :-
{% highlight bash %}
convert -density 600 input.pdf -quality 100 output.png
{% endhighlight %}

Here,
PNG, JPG or (virtually) any other image format can be chosen.<br>
   -density xxx</code> will set the dpi to xxx (common are 150, 300 and 600)<br>
   -quality xxx</code> will set the compression to xxx for PNG, JPG and MIFF file formates (100 means no compression)

--> all other options (such as trimming, grayscale, etc) can be viewed on the website of <a href="http://www.imagemagick.org/script/command-line-options.php">Image Magic</a>.


Feel free to get in touch with the developers(not me!) if you get in trouble.

