---
published: true
---
Real interview problems tend to differ from standard leetcode/hackerrank/glassdoor problems. The interviewer tries to change the assumption of the problems, or restrict the memory of computation. Sometimes they change the input or constraints to make it look difficult. But generally the problem you are given can be solved in the time you are allotted, say 15 minutes. That's all you will get, so you have to plan accordingly; make best use of your time.

### Coding Interview
1. Get comfortable with whiteboard coding. USA
1. Given two strings find the minimum number of characters that needs to be changed to make them anagram. Assume the strings are always of same length. Write a simple driver function with test cases. JFK
1. Nearest letter distance in a word or sentence. Write a function that returns the minimum distance to the nearest occurance of a letter for all letters in the word/sentence. Example: nearestChar("STATES", "S"), should resturn distance to nearest S for every location. In this case the output is [0,1,2,2,1,0]. How do you optimize if you know that the input is always lowercase? i use, key-->ord(ch.lower())-ord('a') to reduce distance calculation time when using hashmap based approach. JFK
1. Find the perimeter of an island for a given point with coordinate _{x,y}_ and an island expressed as a 2-d map of _m_ rows and _n_ columns. **Followup**: you cannot change the input map of island. Matrix is too big, so a separate 2d array of mxn size to keep track of what is visited or not, is not possible. How can we use raster scan to reduce the search space? JFK
1. Assume there is a data structure where a node has two branches that goes down or right, such that down is traversed first then right. Write an algorithm flatten this data structure and return the nodes as a list by following the traversal order. JFK/MIA.
1. T9 Problem: Design phone number to possible words conversion. SEA
1. Employee subordinate count problem: If you have a data structure that represents each person as an employee. Assume each employee has subordinates presented in the same data structure. Return the number of employees or subordinates under each employee. SEA
1. Write a function that can decide whether a word is in a dictionary or not. Return True/False. Assume that the dictionary is implemented in any data structure of your choice. **Followup**: Did you implement it using a efficient data structure? Can we reduce the total space required? Write an algorithm to do range searches such as finding all the words between two words _hello_ and _world_? SEA
1. Write a method to store numbers that are coming from a data stream to some data structure. Write anither function to query how many numbers in that data structure that are smaller than a given value _X_. Try to do it in O(log n)time. SEA/MIA
1. Write BFS/DFS. SEA/AUS/MIA
1. You are given some intervals[(2,6),(4,7), (5,9)]. Return the number that appears in most number of intervals. In this case that number would be 4 or 5 because they appear in all three intervals.




#### System Design
1. Design TinyURL such as bit.ly. URL shortening
1. Design Advertisement System : Write API to find analytics such as users between a certain age group.
