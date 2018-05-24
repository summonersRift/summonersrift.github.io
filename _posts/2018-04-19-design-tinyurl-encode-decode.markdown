---
layout: post
title:  "Design tinyurl encode decode python"
date:   2018-04-19
---

<p class="intro"><span class="dropcap">D</span>esign tinyUrl..</p>
TinyURL is a URL shortening service.  You enter a URL such as https://summonersrift.github.io/blog/linux-large-file-search-ubuntu/  and it returns a short URL such as http://tinyurl.com/R3iAsk.

Leetcode 535 has the same problem. 

Design the encode and decode methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

<b>Answer:</b>Encode-1. The main is to get a fixed length (say 6 characters) random string in "0-9A-Za-z".
<br>Encodde-2. If the entry is already in databse or python dictionary/hasmap, you can use a while loop. until a shortened URL found that doesnt exist in dictionary.
<br>Decode: retrieve the key/last part of url. THen just return the yourmap[shorturl.split("/")[-1]].
<br>


Tnyurl Code—
{% highlight python %}
import random
class Codec:
    
    shortlong = {}
    def encode(self, longUrl):
        """Encodes a URL to a shortened URL.
        
        :type longUrl: str
        :rtype: str
        """
        # we need a list of all characters in 0-9A-Za-z
        chars = [chr(d) for d in xrange(ord('0'), ord('9')+1)]
        chars.extend([chr(d) for d in xrange(ord('A'), ord('Z')+1)])
        chars.extend([chr(d) for d in xrange(ord('a'), ord('z')+1)])
        
        length = 6          # a length of tinyurl you would like, can be determined
        ans = ""             # this is what we will reply
        	     
        for i in xrange(length):   #get 6 random characters from "chars"
            ans += chars[random.randint(0,61)]
        #we can setup a while loop here to see if the short url exists in map/database

        #add url to dictionary or insert into database--if not exists
        self.shortlong[ans]=longUrl
        #print "ans =",ans," long =",longUrl
        return "https://tinyurl/"+ans        

    def decode(self, shortUrl):
        """Decodes a shortened URL to its original URL.
        
        :type shortUrl: str
        :rtype: str
        """
				last = shortUrl.split("/")[-1]
        #print "   short=",shortUrl,"  last=",last
        #print "   long",self.shortlong[last]
        return self.shortlong[last]
{% endhighlight %}


<h4></h4>
