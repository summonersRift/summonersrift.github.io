---
layout: post
title:  "C Strings"
date:   2017-03-01
---

<p class="intro">Working with  strings in C. C doesnt have explicit string data type. We can pass strings as arguments. To save the string, we must create a new buffer and copy the value in the new buffer. In this example we also see how integers and floats can be serialized into a character buffer using <i>snprintf</i>. Here is a code sample-</p>

{% highlight c %}
#include<stdio.h>
#include<malloc.h>
#include<string.h>

char aedemvname_copy[64];

void aedem_kernel(int kid, char* aedemvname, char* aedemvvalue){

   printf("\naedem_nkernel");
   printf("\n  old aedemvname_copy:%s", aedemvname_copy);
   memcpy(aedemvname_copy, aedemvname, sizeof(aedemvname_copy));
   printf("\n  new aedemvname_copy:%s", aedemvname_copy);

   //copy_block_uid = (char*) malloc(sizeof(char) * (strlen(block_uid) + 1));
   //   //strcpy(copy_block_uid, block_uid);
   printf("\n  aedem_kernel=%d aedemvname=%s aedemvvalue=%s", kid, aedemvname, aedemvvalue);
}

int main(){
   int n = 16;
   float err= 0.000001;
   //char varname[64];
   char aedemvvalue[64];

   snprintf(aedemvvalue, sizeof aedemvvalue, "%d", n);
   aedem_kernel(0, "n", aedemvvalue);
   printf("\nn=%s", aedemvvalue);

   snprintf(aedemvvalue, sizeof aedemvvalue, "%f", err);
   aedem_kernel(1, "err", aedemvvalue);
   printf("\nerr=%s", aedemvvalue);

   printf ("\n");
   return 0;
}
{% endhighlight %}

<br>
Compile: gcc -o execfile filename.c <br>
Run: ./execfile
<br>

<b>Output—</b>
{% highlight bash %}
aedem_nkernel
  old aedemvname_copy:
  new aedemvname_copy:n
  aedem_kernel=0 aedemvname=n aedemvvalue=16
n=16
aedem_nkernel
  old aedemvname_copy:n
  new aedemvname_copy:err
  aedem_kernel=1 aedemvname=err aedemvvalue=0.000001
err=0.000001
{% endhighlight %}
