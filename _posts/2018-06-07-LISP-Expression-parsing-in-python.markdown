---
layout: post
title:  "Pre-order LISP Expression Parsing"
date:   2018-06-07
---

<p class="intro"><span class="dropcap">E</span>xpression parsing in Python-</p>
Suppose you are given an expression (in language like LISP) where operator precedes the operands or a pre-ordering of operations. (* 6 7) means 6*7. 
OR something like: (* 5 (+ 2 3)). You are asked to write a program that outputs the result of the expression, 25 for the second case.
LISP programming language uses this kind of prefix notation for expressiong expressions. 
For the problem you can assume that the input statement is valid. 


There are many ways to do it.

<b>(Path-1)</b> You can split the string by <b>(</b> and then split by <b>)</b>. One by one.
Finally you have something like ["* 5","+ 8 7",""]. 
Now what you have to do is, concatenate first item with last to maintain the precedence.
Then we can reverse the list, evaluate the first item(pop+evaluate) 
and concatttenate the result with the next index <b>i+1</b>. Finally we will have our results

<b>Path-2</b> There is even a better way to solve this problem. 
Remember in compiler design or data structures we simply push the operators and operands in the stack.
When we encounter a matching closing <b>)}]</b> we pop everything until the last <b>({[</b>.
Check the following code. Might have to add spaces in <b>pre-processing</b>.

<b>Solution— </b>
{% highlight python %}

text = "( + 5 ( * 3 4 2 ) 5 )".split(" ")

stack = []
ops = "*+-/"
opening = "({["
closing = ")}]"
result = 0
for i, ch in enumerate(text):
    print ch
    if ch in opening:
        stack.append(ch)
    elif ch in ops:
        stack.append(ch)
    elif ch in closing:
        loop = True
        operands = []
        op = None
        val = 0
        while loop:
            item = stack.pop(-1)
            print "--item",item
            if item in opening:
                loop = False
            elif item in ops:
                op = item  #discard the opening bracket/parenthesis
            else:
                operands.append(item)
        operands = [int(d) for d in operands]
        if op == "+":
            val=  sum(operands)
        elif op == "*":
            val = 1
            for d in operands:
                val *=d
        
        stack.append(str(val))
        result = val
    else: #operand
        stack.append(ch)
        
print result
{% endhighlight %}


<h4>Output— </h4>
{% highlight python %}
sh-4.4$ python main.py
(
+
5
(
*
3
4
2
)
item 2
item 4
item 3
item *
item (
5
)
item 5
item 24
item 5
item +
item (
34
{% endhighlight %}



The practice output code is generated online using tutorialspoint free online IDE for python. 
https://www.tutorialspoint.com/online_python_ide.php
