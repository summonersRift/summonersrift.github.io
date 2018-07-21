---
layout: post
title:  "Python internal data staructures"
date:   2018-07-18
---

<p class="intro">Python has different implementations, <i>CPython</i> is the most popular one. </p>
<ol>
  <li><b>List—</b> Lists are implemented as C like arrays in consecutive memory blocks.
      <ul>
        <li><b>.append(x)</b>: O(1) complexity, since appending at the end of list contents. </li>
        <li><b>.insert(index, val)</b>: O(n) complexity, append at the specific index and then SHIFT everything after it by 1 index.</li>
        <li><b>.pop()</b>: O(1) complexity, pops the value of last item in the list. </li>
        <li><b>.pop(index) or, remove(value) </b>: O(n) complexity, delete the <i>value</i> and shift other items up. </li>
        <li><b>List growing optimization </b>: Python lists initialized with no data then it grows in a particular order using <b>list_resize()</b>. To reduce total number of list_resize () calls which allocates new memory the growth happens in the following order 0, 4, 8, 16, 25, 35, 46, 58, 72, 88, …</li>
        <li><a href="https://www.laurentluce.com/posts/python-list-implementation/">Detailed reference</a></li>
      </ul>

  </li><br>
  <li><b>Dictionaries—</b>Implmented using hash tables. Steps in dictionary appending
      <ul>
        <li><b>hash(value)</b>: Find the hash of the value using any hash function such as <i>%52</i> for strings represented as their ascii value.  </li>
        <li><b>hash(key) &amp;	7 </b>: finds an address <=7. Such as if hash(a)=1; hash(a)&amp;7 is 1; So list[1]=value. </li>
        <li><b>Collision</b>: When a address has the value. Use a collision resolve method. Standard is <i>open addressing</i>. It could be a lined list, or better a BST on that particular value. </li>
        <li><b>Growing</b>: When the total entries are over 2/3 of the array’s capacity. dict_resize() is called to allocate a larger array. This function also <b>copies</b> the old table entries to the new table. To reduce the number of resize steps and increase sparseness it is . The new table size needs to be greater than 24 and it is calculated by shifting the current size 1 bit left until it is greater than 24. It ends up being 32 e.g. 8 -> 16 -> 32. Read more <a href="https://www.laurentluce.com/posts/python-dictionary-implementation/">here</a>. TO summarize a new table of size 32 is allocated. Old table entries are inserted into the new table using the new mask value which is 31.</li>
      </ul>
  <li><b>Tuples—</b>Tuples </li> </ol>


<br><br>
<b>List  object C structure—</b>
{% highlight c %}
typedef struct {
    PyObject_VAR_HEAD
    PyObject **ob_item;
    Py_ssize_t allocated;
} PyListObject;
{% endhighlight %}


<br><br>
<p><b>My refernces—</b>
1. https://www.laurentluce.com/posts/python-list-implementation/<br>
2. https://www.laurentluce.com/posts/python-list-implementation/<br>


