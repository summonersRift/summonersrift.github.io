---
layout: post
title:  "Programming Garbage Distance"
date:   2017-01-01
---


<p class="intro">There is a robot that collects garbage from a 2-D map of size <b>m x n</b>, where m is number of rows and n is number of columns. A robot starts at top-left (0,0) cell of the map and tries to find the shortest distance to a garbage. A cell with garbage has a value 9. There are obstacles on the map that is marked as 0, meaning robot cant visit or pass through that cell. For all other values the cell is accesible. Find the shortest distance to the garbage, in terms of moves needed to get to the garbage.</p>

#### Conditions
* You cant modify the original map.
* The program must not create an array to track the entire map. 
* The robot can add a new entry to a data structure for a cell it visits. On the fly is allowed.
* The garbage is guaranteed to be in the map.


#### Hints
* Use raster scan to build cell unique ids as you go.
* Keep track of the unique ids traversed with a data structure that has O(1) expected insert and search time.
* Start from 0,0 and use BFS to add the neighbors that are not visited.

### Solution:
{% highlight c linenos%}
def minimumGarbageDistance(lot):
    nodes = [[0, 0, -1]] #x,y, distance
    checked = set()
    m = len(lot)
    n = len(lot[0])

    while nodes:
        node = nodes.pop(0)
        x, y, d = node
        
        if 0<=x<m and 0<=y<n:
            uid = n*x + y
            if uid not in checked:
                checked.add(uid)
                newd = d + 1
                if lot[x][y] == 9: #garbage
                    return newd
                elif lot[x][y] == 1:  #flat landscape
                    nodes.append([x, y+1, newd])
                    nodes.append([x, y-1, newd])
                    nodes.append([x+1, y, newd])
                    nodes.append([x-1, y, newd])
                #else obstacle or 0
    return -1

if __name__=="__main__":   #test driver program 
   lot = [
            [1,1,1,1],
            [1,1,1,1],
            [0,0,0,1],
            [1,1,9,1]
         ]
   print minimumGarbageDistance(lot)

{% endhighlight %}


<p class="intro"><b>Problem</b>: If you have multiple such robots or 1 robot and multiple ghosts, find who gets to the garbage first. You are given with a player coordinate and multiple ghosts coordinate inside the map. </p>
Implement: findWinner (lot, player, ghosts) <br> 
Output: returns True if player gets to the garbage or False when ghost gets to the garbage first.
<br>
Do it in O(mn)
<br><br>
Cheers!
