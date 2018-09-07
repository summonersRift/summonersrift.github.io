---
published: true
---
Real interview problems tend to differ from standard leetcode/hackerrank/glassdoor problems. The interviewer tries to change the assumption of the problems, or restrict the memory of computation. Sometimes they change the input or constraints to make it look difficult. But generally the problem you are given can be solved in the time you are allotted, say 15 minutes. That's all you will get, so you have to plan accordingly; make best use of your time.

### Coding Interview
1. Get comfortable with whiteboard coding. USA
1. Medical records are stored in a certain way with dashes in between. You are given an integer number 234553353433, Find Medical record number MRN split by dashes in the following format that will be given as input [4,3,4,1]. Return “2345-533-5343-3”. WISC/MIA
1. Add two integer numbers. A = [3,4,5,6,1] and B= [5,4,1], Return A+B as a a list. Most significant digit is the first digit (0-th) on the list. SAME
1. A typist puts his finger on a keyword to type. FIngers are numbered 0 to 9.  0=null, 1= v,c,b,g, 2=...., 5=null, 9=r,t,e. Find all the permutations for a given number say 8342. SAME 
1. You are given a dictionary in a NxN matrix and a keyword word to search in the dictionary. The word may appear in a in a column, inversely backward in a row and a column. Find the occurrence and return the grid positions as [[x1,y1], [x2,y2],....[xn,yn]]. Find all such occurrences. SAME
1. Write an expression parser for an expression the following form: (+ (* 3 4) 5). Extend this to handle multiple operands such as (* 3 4 5 6) means _3*4*5*6_.
SFO/MIA
1. Given two strings find the minimum number of characters that needs to be changed to make them anagram. Assume the strings are always of same length. Write a simple driver function with test cases. JFK
1. Nearest letter distance in a word or sentence. Write a function that returns the minimum distance to the nearest occurance of a letter for all letters in the word/sentence. Example: nearestChar("STATES", "S"), should resturn distance to nearest S for every location. In this case the output is [0,1,2,2,1,0]. How do you optimize if you know that the input is always lowercase? i use, key-->ord(ch.lower())-ord('a') to reduce distance calculation time when using hashmap based approach. JFK
1. Find the perimeter of an island for a given point with coordinate _{x,y}_ and an island expressed as a 2-d map of _m_ rows and _n_ columns. **Followup**: you cannot change the input map of island. Matrix is too big, so a separate 2d array of mxn size to keep track of what is visited or not, is not possible. How can we use raster scan to reduce the search space? JFK
1. Assume there is a data structure where a node has two branches that goes down or right, such that down is traversed first then right. Write an algorithm flatten this data structure and return the nodes as a list by following the traversal order. JFK/MIA.
1. T9 Problem: Design phone number to possible words conversion. SEA
1. Employee subordinate count problem: If you have a data structure that represents each person as an employee. Assume each employee has subordinates presented in the same data structure. Return the number of employees or subordinates under each employee. SEA
1. Write a function that can decide whether a word is in a dictionary or not. Return True/False. Assume that the dictionary is implemented in any data structure of your choice. **Followup**: Did you implement it using a efficient data structure? Can we reduce the total space required? Write an algorithm to do range searches such as finding all the words between two words _hello_ and _world_? SEA
1. Write a method to store numbers that are coming from a data stream to some data structure. Write anither function to query how many numbers in that data structure that are smaller than a given value _X_. Try to do it in O(log n)time. SEA/MIA
1. Write BFS/DFS. SEA/AUS/MIA
1. You are given some intervals[(2,6),(4,7), (5,9)]. Return the number that appears in most number of intervals. In this case that number would be 4 or 5 because they appear in all three intervals. SFO/SEA/MIA.




#### System Design
1. Design TinyURL such as bit.ly. URL shortening
1. Design Advertisement System : Write API to find analytics such as users between a certain age group.
