---
published: true
---

Lets look at some gnuplot examples.

#### Code
{% highlight bash%}
set terminal pdf enhanced linewidth 2
set output "jacobi-real-vs-aedem-resource-usage.pdf"

#set logscale y
#set logscale x
set log xy           #set logscale at the same time
set yrange [*:*]
set xrange [*:*]     # to get a smart range
#set ytics nomirror  # to disable mirroring on second y axis
unset mytics         # disable minor ytics

set xtics (64,128,256,512,1024,2048,4098,8192,16384,32768) nomirror #xtics at scecific numbers

set key bottom right
set title "Jacobi Iterative Method Resource Usage predictions"
set ylabel "Total Usage"
set xlabel "Input Size"

set ylabel "Num of Instructions"
plot "jacobi-original.dat" using 1:2 with linespoints pt 2 dt '.' title "real",\
 "jacobi-predictions.dat" using 1:2 with linespoints pt 6 ps 1.3 dt 2 lc rgb "black" title "predicted(aedem)"

{% endhighlight %}


<blockquote> {% highlight bash%} code goes here {% endhighlight %} </blockquote>
