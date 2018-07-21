---
layout: post
title:  "Python internal data structures"
date:   2018-07-18
---

<p class="intro">Python has different implementations, <i>CPython</i> and Python-JIT are popular ones. </p>

<ol>
  <li><b>List—</b> Lists are implemented as C like arrays in consecutive memory blocks.
      <ul>
        <li><b>.append(x)</b>: O(1) complexity, since appending at the end of list contents. </li>
        <li><b>.insert(index, val)</b>: O(n) complexity, append at the specific index and then SHIFT everything after it by 1 index.</li>
        <li><b>.pop()</b>: O(1) complexity, pops the value of last item in the list. </li>
        <li><b>.pop(index) or, remove(value) </b>: O(n) complexity, delete the <i>value</i> and shift other items up. </li>
        <li><b>List growth optimization </b>: Python lists initialized with no data then it grows in a particular order using <b>list_resize()</b>. To reduce total number of list_resize () calls which allocates new memory the growth happens in the following order 0, 4, 8, 16, 25, 35, 46, 58, 72, 88, …</li>
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
  </li><br>

  <li><b>Tuples—</b>Tuples </li> 
  <li><b>Sets—</b>Sets </li> </ol>


<br><br>
<b>List  object C structure—</b>
{% highlight c %}
typedef struct {
    PyObject_VAR_HEAD
    PyObject **ob_item;
    Py_ssize_t allocated;
} PyListObject;
{% endhighlight %}


An apple guy once asked me what are two fundamental data structures? The answer he was looking for is <b>stack</b> and <b>queuue</b>. 


<h3> Python Data Structures </h3>
1. Python data structures include lists, dictionaries, tuples, and sets.
1. Python lists are implemented internally as variable-length arrays, rather than linked lists.
1. Python dictionaries are implemented internally as resizable hash tables.
1. Python tuples are records.
1. Python sets are implemented internally as dictionaries (hash tables) with keys and no values.
1. The data types for a list, dictionary, tuple, and set, are 'list', 'dict', 'tuple', and 'set', respectively.
1. The list(), dict(), tuple(), and set() functions may be used to create list, dictionary, tuple, and set objects, respectively.
1. List and dictionary items are mutable. Tuple and set items are immutable.
1. Lists and tuples maintain order. Dictionaries and sets are unordered.
1. Lists and tuples allow duplication. Dictionary and set items are unique.
1. Sets may be used to remove duplication found in other data structures.
1. List, dictionary, tuple, and set items may be accessed using a for loop.
1. List and tuple items may be accessed by index. Dictionary items are accessed by key. Set items cannot be accessed by index.
1. Python supports having a tuple on the left side of an assignment statement. This allows you to assign more than one variable at a time.
1. Sets support logical set operations, including membership, subset, superset, union, intersection, and difference.
1. Reference: <a href="https://en.wikiversity.org/wiki/Python_Programming/Tuples_and_Sets"> wikiversity Python data structure summary.</a>


<h3> Data Structure Concepts</h3>
1. A data structure is a particular way of organizing data in a computer so that it can be used efficiently.
1. Data structures can be used to organize the storage and retrieval of information stored in both main memory and secondary memory.
1. The array and record data structures are based on computing the addresses of data items with arithmetic operations; while the linked data structures are based on storing addresses of data items within the structure itself.
1. An array is a number of elements in a specific order, typically all of the same type. Elements are accessed using an integer index to specify which element is required. Arrays may be fixed-length or resizable.
1. A linked list is a linear collection of data elements of any type, called nodes, where each node has itself a value, and points to the next node in the linked list.
1. A record (also called tuple or struct) is an aggregate data structure. A record is a value that contains other values, typically in fixed number and sequence and typically indexed by names. The elements of records are usually called fields or members.
1. A class is a data structure that contains data fields, like a record, as well as various methods which operate on the contents of the record.



<p><br>
<b>My refernces—</b><br>
1. <a href="https://www.laurentluce.com/posts/python-list-implementation/">https://en.wikiversity.org/wiki/Python_Programming/Tuples_and_Sets</a>
2. <a href="https://en.wikiversity.org/wiki/Python_Programming/Tuples_and_Sets"> https://www.laurentluce.com/posts/python-list-implementation/</a>
1. <a href="https://en.wikiversity.org/wiki/Python_Programming/Tuples_and_Sets">https://en.wikiversity.org/wiki/Python_Programming/Tuples_and_Sets a wikiversity</a>


