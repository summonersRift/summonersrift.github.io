---
published: true
---

<figure>
	<img src="{{ '/assets/img/einstein_coding_interview.jpg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Believe in yourself.</figcaption>
</figure>


### Resources
1. [MIT CSAIL - Hacking a google interview ][hacking-a-google-interview]
2. [Leetcode - Top interview problems][leetcode-top-interview-problems]: my favorite list of problems to become a ninja. In otherwors words, this gets you in shape.
    1. Another Nested Item
3. [Pramp - peet-to-peer interview][join-pramp] I took quite a few peer-to-peer interviews on pramp. Its a platform where you interview a peer for 30 minutes and then they interview you. Half of the time they are bette than you and rest of the times the interviewers dont have much experience. Regardless they have great question, followup material and a nice framework that gives you a good exposure to what the phone interviews are like.
4. [Eric Grimson, MIT,  Recursion Class][eric-grimson-recursion]: a very useful resource on recursion.
6. [Palantir - The Coding Interview Steps][palantir-coding-interview] sidenote: The Einstein photo in this post comes from this post.
7. On System Design 
   1. [Palantir - System Design Interview Guide][palantir-system-design-interview]: a brief blog. What companies expect of a candidate in a system design interview.
   2. [Grokking the System Design Interview][grokking-system-design] : an online course on system design. Includes all major systems. 80$, life-time  access, totally worth it.
   3. [Twitter system design on Hackernoon by Fahimul Haq][twitter-system-design]
   4. [Uber system design][uber-system-design]
   5. [My System design post][obaida-system-design]

### Approaching an Interview, Step by Step
1. **Make sure you understand the problem**: ask clarifying questions, until you understand the problem. If you are not clear from the description ask the interviewer to give you a test case. What happens is that interviewers generally start with a test case then talk about the problem.
2. Think of a  _bruteforce_ solution (think loud): Talk about the your understanding of the problem, how you are approaching it. And that you will try a optimal if you are not sure you can pull the optimal off right away.
3. Dont code until you and the interviewer have agreed on the solution or approach.
4. Pick a language. Code the agreed solution. TIme to show your ninja skillsTalk about what you are doing or trying to achieve when you are coding a specific part.
5. Find _optimal_ solution if you can. Eliminate boundary cases.
6. Walk through a test case to convice your interviewer that the solution works. 

#### Some good practices:
* You can assume simple helper functions are available and you will code it later if time allows.

#### Don't
* Don't Start coding right away.



### Topics
* Tree and Tries: know basic implementations
* String Searching
* Graph shortest path
* Dynamic: breaking OPTIMAL(N) = fn {X + OPTIMAL(N-1)}
  * fn can be min/max or anything else

<figure>
	<img src="{{ '/assets/img/matrix_hallway.jpg ' | prepend: site.baseurl }}" alt=""> 
	<figcaption>You are the master architect of your coding interview.</figcaption>
</figure>

<blockquote>Things in your control are how you prepare and tackle follow up questions. Decision will be made by the interviewers, no reason beating yourself up if you dont make it this time. You will, next time. Cheers!</blockquote>

### My Python Cheatsheet of everything
{% highlight python%}
Question: Lambda sort by second value of a list
>>> a = [[1,2], [0,1], [4,3], [4,5], [3,1]]
>>> a = sorted(a, key = lambda x:x[1])
>>> a
[[0, 1], [3, 1], [1, 2], [4, 3], [4, 5]]

Question: How to sort a dict by keys
mydict = {'carl':40, 'alan':2, 'bob':1, 'danny':3}
keylist = mydict.keys()
keylist.sort()
for key in keylist:
    print "%s: %s" % (key, mydict[key])
alan: 2
bob: 1
carl: 40
danny: 3

Question: Sort a dict by value
for key, value in sorted(mydict.iteritems(), key=lambda (k,v): (v,k)):
    print "%s: %s" % (key, value)
bob: 1
alan: 2
danny: 3
carl: 40

Question: ASCII <---> char
>>> ord('a') 
97
>>> chr(97) 
'a'
>>> chr(ord('a') + 3) 
'd'

Question: Reverse any list or string,  s
>>> s = "abc"     a = [1,2,3,4]
>>> s[::-1]       a= a[::-1]
'Cba'    
>>> s             a
'Abc'             [4,3,2,1]

Question: Reverse a list with reversed function:
>>> a = ["foo", "bar"] 
>>> for i in reversed(a): 
      print i 
Bar
Foo

WARNING! : altering string characters by index is not allowed. replace(“src”,”dst”) allowed.
To do that we have to convert s to a list then operate on it.
ret = list(s)
ret =  ret[::-1] #or, reversed(ret)  # ALT: use while to swap list items s[i] <--> s[n]
ret = "".join(ret)
return ret

Removing a key from dictionary  → a.pop(k, None)
my_dict.pop('key', None)


Question: Can we have integers as keys to Dictionaries? yes: 
>>> a = {1:10,2:"val2","key3":30}
>>> a
{'key3': 30, 1: 10, 2: 'val2'}
>>> a[1]
10
>>> a['key3']
30

Question: xrange-for (is faster than)  enumerate-for (is much faster than) pop-while
$ python -m timeit  'a = []' 'for i in xrange(10000): a.append(i)' 'for i in a: x=i'     →    1000 loops, best of 3: 1.06 msec per loop
$ python -m timeit  'a = []' 'for i in xrange(10000): a.append(i)' 'for index, val in enumerate(a):x=val'
1000 loops, best of 3: 1.27 msec per loop
$ python -m timeit  'a = []' 'for i in xrange(10000): a.append(i)' 'while a: x = a.pop(0)'       →   100 loops, best of 3: 15.4 msec per loop

Question: Python hash calculation
hash("ab")
>>> 12416074593111939
>>> hash(1)
>>> 1
>>> map(hash, (0, 1, 2, 3))
[0, 1, 2, 3]
>>> map(hash, ("namea", "nameb", "namea"))
[-1658398457, -1658398460, -1658398457]
>>> map(hash, ("ab", "ba", "cd", "ab"))
[12416074593111939, 12544075362113221, 12672076131114255, 12416074593111939]
https://www.laurentluce.com/posts/python-dictionary-implementation/

Question: Character to integer, lower() and discard special characters
>>> ord(‘A’) is 65            ord(‘Z’) = 90
>>> ord(‘a’) is 97            ord(‘z’) = 122
>>> for i, ch in enumerate("Word.Ok!".lower()):
...  if ch >='a' and ch <='z':
...    print ch
w
o
r
d
o
k
Collections/Counter, # occurrences of words in a document or,  list of words
>>> from collections import Counter   # remember Counter not count
>>> cnt = Counter()
>>> for word in ['red', 'blue', 'red', 'green', 'blue', 'blue']:
...    cnt[word] += 1
>>> cnt
Counter({'blue': 3, 'red': 2, 'green': 1})

Question: >>> # Find the ten most common words in Hamlet
>>> import re
>>> words = re.findall(r'\w+', open('hamlet.txt').read().lower())
>>> Counter(words).most_common(10)
[('the', 1143), ('and', 966), ('to', 762), ('of', 669), ('i', 631),
 ('you', 554),  ('a', 546), ('my', 514), ('hamlet', 471), ('in', 451)]

>>> mydict = {'carl':40, 'alan':2, 'bob':1, 'danny':3,'don':3}
    >>>
>>> sorted(mydict.iteritems(), key=lambda (k,v): (v,k)) 
[('bob', 1), ('alan', 2), ('danny', 3), ('don', 3), ('carl', 40)]

You only need to return the value that you are comparing in the lambda, as opposed to flipping the key and value in a tuple

>>> sorted(mydict.iteritems(), key=lambda (k,v): v)
[('bob', 1), ('alan', 2), ('danny', 3), ('don', 3), ('carl', 40)]

Checking if a object is an instance of a class:
if isinstance(obj, MyClass):
     print "obj is my object"

Question: Data type conversion
→ reversing a integer number in 32 bit and return back
n = 14
b = bin(n)  #binary of ‘a’ -- “0b1110”
# also have hex, oct for hexadecimal and octal
#OR b= b.replace(“0b”,“”)
dummy = [“0” for i in xrange(32-len(b) )]
b = list(b)  # make list
b  = dummy + b
b = b[::-1]  # reverse
b = “”.join(b)
#convert back to integer and return
return int(b,2) #if there is 0x--hex or 0b--bin we can just replace it for operation

Large Numbers
If you just need a number that's bigger than all others, you can use
>> float('inf')

in similar fashion, a number smaller than all others:
>> float('-inf')

The value sys.maxint, which is either 231 - 1 or 263 - 1 depending on your platform.

→ googol
>>> 10**100
10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000L

>>> a = 10**100
>>> b = sys.maxint
>>> if a >b:
...   print "googol is beggier than maxint"
...
... googol is beggier than maxint

>>> long(1234567891011121314151617181920)
>>> 1234567891011121314151617181920L

>>> a = sys.maxint
>>> b = float("inf")
>>> if b>a:
...   print "inf is bigger"
... else:
...   print "maxint is bigger"
...
... inf is bigger


Randoms
import random

>>> random.random()  # gives a random  0<=n<=1
0.010835814926994924

>>> random.randint(a,b)  # gives a random  a<=n<=b

* If a seed is set then you will get the same result right after setting the seed
random.seed(a)  # a is integer


Question: Bitwise operations
a = 60            # 60 = 0011 1100 
b = 13            # 13 = 0000 1101 
c = 0

c = a & b;        # 12 = 0000 1100    AND
c = a | b;        # 61 = 0011 1101    OR
c = a ^ b;        # 49 = 0011 0001    EX-OR
c = ~a;           # -61 = 1100 0011    NOT
c = a << 2;       # 240 = 1111 0000    Shift left twice, with 0 from right
c = a >> 2;       # 15 = 0000 1111    Shift Right twice, with 0 from left
Singly linked list palindrome: 
Choice 1: make a string list and push by traverse then match .
Choice 2: Solve in O(n) time and O(1) extra space.
1) Get the middle of the linked list by getting length and going to mid node. 
2) make a new pointer node after the middle (even/odd index matters)
4) reverse second half.
3) traverse both the list. Check if the first half and second half are identical.
------
4) Construct the original linked list by reversing the second half again and attaching it back to the first half

Copy a list in python--
a =[1,2,3]
b= a[:]
#OR
b= list(a)

Set operations in python:
myset.add(data)
myset.delete(data)
x = myset.pop()  # can only pop first element or raise exception

Distance between two points,  d =  (x1-x2)2 + (y1-y2)2 
Area of a triangle:  ½ *length*height
import math
S = (a+b+c) / 2.0  #half of the triangles perimeter
#Area =  S*(S-a)*(S-b)*(S-c) 
area = math.sqrt (s*(s-a)*(s-b)*(s-c))

Two points  (x1, y1) and (x2, y2), slope is   y2- y1x2- x1
Roman numerals: I=1; X =10, V=5 L=50, C=100, D =500, M=1000
http://literacy.kent.edu/Minigrants/Cinci/romanchart.htm
roman to integer.
    a) convert str to a list. b) convert each character to integer equivalent
            c*) if digits[i] >digits[i-1]: digits[i] =-1*digits[i]       d)return sum(digits)

Python Strings
Creation
the_string = “Hello World!”
The_string[4]
#returns ‘o’


Split
the_string.split(‘ ’)   → nothing in ‘’ means space ‘ ’
[‘Hello’, ‘World!’] 

Replace words/substrings
str2 = the_string.replace(‘Hello’, “OK”)
“OK World!”

Join
words = [“this”, ‘is’, ‘a’, ‘list’, ‘of’, “strings”]
ret = ‘’.join(words)
#This is a list of strings
‘ZOOL’.join(words)
#ThisZOOLisZOOLaZOOLlistZOOLofZOOLstrings
‘’.join(words)
#Thisisalistofstrings

Print "codecodecodecode mentormentormentormentormentor" without using loops
print "code"*4+' '+"mentor"*5

To count occurance of a characeter, use  count(), returns 
if s.count('(')!=s.count(')') or s.count('[')!=s.count(']')




Counter Method
from collections import Counter
def is_anagram(str1, str2):
     return Counter(str1) == Counter(str2)
>>> is_anagram('abcd','dbca')
True
>>> is_anagram('abcd','dbaa')
False

Substring find:         str.find(what, beg=0, end=len(string))
what − search string beg − beginning index, [default 0, end − ending[default=n-1]
Return : - index if found and   -1  otherwise.

str1 = "this is string example....wow!!!";
str2 = "exam";
print str1.find(str2)
print str1.find(str2, 10)
print str1.find(str2, 40)
15
15
-1

Find all occurances of a substring
import re
[m.start() for m in re.finditer('test', 'test test test test')] → iteratively what, where
#[0, 5, 10, 15]
>>> [[m.start(),m.end()] for m in re.finditer('test', 'test test test')]
>>> [[0, 4], [5, 9], [10, 14]]

If you want to find overlapping matches, lookahead will do that:
[m.start() for m in re.finditer('(?=tt)', 'ttt')]
#[0, 1]

Python Tuples
A tuple consists of a number of values separated by commas. They are useful for ordered pairs and returning several values from a function.
Creation: 
emptyTuple = () 
singleItemTuple = (“spam”,)
thistuple = 12, 89, 'a'
thistuple = (12, 89, 'a')               #note the comma

Accessing: 
thistuple[0]    →  returns 12

Python Dictionaries
A dictionary is a set of key:value pairs. All keys in a dictionary must be unique.
Creation:
emptyDict = {}
    mydict = {'a':1, 'b':23, 'c':"eggs"}

Accessing: deleting: finding:
mydict['a']        →  returns 1
del mydict['b']
mydict.has_key('e')  → returns True/False
    
>>> mydict = {'a':1, 'b':2, 3:'c'}
>>> mydict.keys()
['a', 3, 'b']
>>> mydict.values()
[1, 'c', 2]
>>> mydict.items()
[('a', 1), (3, 'c'), ('b', 2)]

Python dictionaries implement a tp_iter slot that returns an efficient iterator of keys.
for k in mydict: …    #equivalent to, but much faster than      for k in mydict.keys(): …
    Example: ret = [key for key in mydict if mydict[key]==True] [0]

Iterate key, value pairs:
i.       for key in dict.iterkeys():   #for x in dict is shorthand for for x in dict.iterkeys().
ii.  for value in dict.itervalues():
iii. for key, value in dict.iteritems(): …
iv.  for item in dict.items():   #python3 and faster than iter items



--> List : one of the most important data structures in Python is the list. Lists are very flexible and have many built-in control functions.

>>> [0]*8
[0, 0, 0, 0, 0, 0, 0, 0]
>>> ['ok'] * 4
['ok', 'ok', 'ok', 'ok']

mylist.count()
mylist = [123, 'xyz', 'zara', 'abc', 123]
print mylist.count(123), mylist.count(99)
-----------
2 0


creation: accessing: slicing:
length: sort: add: return &
remove: insert: remove:
thelist = [5,3,‘p’,9,‘e’]


[5,3,’p’,9,’e’]
[5,3,’p’,9,’e’]
[5,3,’p’,9,’e’]
[5,3,’p’,9,’e’]
[5,3,’p’,9,’e’]
[5,3,’p’,9,’e’]
[5,3,’p’,9,’e’]
thelist.sort()  → no return value

thelist.append(37)
thelist.pop() returns 37
thelist.pop(1) returns 5
thelist.insert(2, ‘z’)
thelist.remove(‘e’)
del thelist[0]


[3,5,9,’e’,’p’]
[3,5,9,’e’,’p’,37]
[3,5,9,’e’,’p’]
[3,9,’e’,’p’]
[3,’z’,9,’e’,’p’]
[3,’z’,9,’p’]
[‘z’,9,’p’]
[‘z’,9,’p’]
[‘z’,9,’p’]


‘c’ in thisdict
‘paradimethylaminobenzaldehyde’ in thisdict returns False
thelist[0]
Thelist[1:3]   similar to [thelist[i] for i in xrange(1,3)]
thelist[2:]
thelist[:2]
thelist[2:-1]
len(thelist)


returns 5 returns [3, ‘p’]
returns [‘p’, 9, ‘e’] returns [5, 3]
returns [‘p’, 9] returns 5
thelist.sort()
thelist.append(37)


no return value
thelist.pop() returns 37thelist.pop(1) returns 5thelist.insert(2, ‘z’) thelist.remove(‘e’)
del thelist[0]concatenation: thelist + [0]
finding: 9 in thelist
returns [‘z’,9,’p’,0] returns True

returns False
returns [‘a’, ‘c’]
returns [(‘a’, 1), (‘c’, ‘eggs’)] 
returns True

List Comprehension: A special expression enclosed in square brackets that returns a new list. The expression is of the form:[expression for expr in sequence if condition] The condition is optional.

A = [d for d in xrange(5) if d%2 == 0]



##### Files
Open:
thisfile = open(“datadirectory/file.txt”, “r”)  # r, w → read,write

Accessing:
infile.read()        → reads entire file into one string 
infile.readline()        → reads one line of a file
lines = infile.readlines()        → reads entire file into a list of strings, one per line
for eachline in infile:    → steps through lines in a file
    print eachline
import os.path 
os.path.isfile(fname)  → returns True or False

{% endhighlight %}


[hacking-a-google-interview]:    http://courses.csail.mit.edu/iap/interview/materials.php
[eric-grimson-recursion]:    https://www.youtube.com/watch?v=WPSeyjX1-4s
[leetcode-top-interview-problems]:    https://leetcode.com/problemset/top-interview-questions/
[join-pramp]:    https://www.pramp.com/invt/B6qyardYOdh9MWbG5mV9
[palantir-system-design-interview]:    https://www.palantir.com/how-to-ace-a-systems-design-interview/
[palantir-coding-interview]:    https://www.palantir.com/the-coding-interview/
[grokking-system-design]:    https://www.educative.io/collection/5668639101419520/5649050225344512
[twitter-system-design]:    https://hackernoon.com/anatomy-of-a-system-design-interview-4cb57d75a53f
[uber-system-design]:    https://sakib.ninja/experimenting-uber-like-application-architecture/
[obaida-system-design]:    https://summonersrift.github.io/blog/System-Design/
