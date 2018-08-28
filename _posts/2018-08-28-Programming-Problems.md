---
published: true
---
In this post, I will list and briefly discuss some important programming problems specially for an interview.

![MIT Advanced algorithm data structure illustration](https://courses.csail.mit.edu/6.851/fall17/illus.png)


#### Tree, BST
- Find the size or depth of the binary tree.
  - Find leaves and internal nodes.
- How pre-, post-, and in-order traversal works?
  - Traverse a given binary tree in preorder without recursion?
- Flattening a binary tree as list of nodes.
- List of nodes at a certain level _k_, where the deepest level in _n_.
- [Finding graph diameter](https://leetcode.com/problems/diameter-of-binary-tree/)

#### Graph:
- Shortest Path


#### String:
1. [Sub-string Search](https://www.youtube.com/watch?v=GTJr8OvyEVQ)
1. Duplicate characters in a string. Similarly, first not repeated character in a string.
1. Intersection of two words. Returns the common characters.
1. Changes need to be made in one string to make it look like another string. Changes means deletes and swaps.
1.  Anagram, palindrome
1. Find all permutations of a string.
1. Reverse a sentence without touching spaces and special characters.


#### Linked-list:
Linked list goes really well with recursion.
- How do you find the middle element of a singly linked list in one pass?
  - Hint: Can be done with two pointers. [[Solution](https://javarevisited.blogspot.com/2012/12/how-to-find-middle-element-of-linked-list-one-pass.html)]
- Find if a linked list is palindrome withour using extra space.
  - Hint: Linked-list reverse.
- [Find sum of two numbes represented as Linked list and return the sum as a linked list.](https://leetcode.com/problems/add-two-numbers-ii/description/)
- Merge two sorted linked lists with recursion.
- Check whether a linked list has a cycle or not.
- Reverse linked list without recursion.
- How to find the the third node from the end in a singly linked list?




### Array, Number and list manipulation
- How do you find the missing number in a given integer array of 1 to N? Canbe done in O(logn) time.
  - Can be done in O(n) if the numbers are not sorted. Hint: use n(n+1)/2
  - Can be done in O(log n) if the numbers are sorted.
- If a list contains some numbers exactly twice except for one number, find the number that occurs once. 
  - Use bitwise operation to solve in O(n) time and O(1) space complexity.
  - can uyse python set with add/remove, but would be O(n) time and space complexity.
- Swapping two numbers without using a temporaryk memory or variable.
- Merge some intervals. Merging [[1-5], [4-6],[3-9], [8-14], [10-12], [16-19]] returns [[1-14], [16-19]].
  - Follow-up: How to find a number that occurs the most in these intervals. 
- Find and possibly remove the duplicate number from a given integer array? Can you do it w/o using library. 
- Find 3-SUM where sum of any 3 numbers in a list of numbers equals to K. Do it in O(n^2).
- Quicksort in place.
- Implement merge sort, bucket sort, 
- Given an array containing both negative and positive integers. Find the contiguous sub-array with maximum sum. [KADANE's algorithm](https://practice.geeksforgeeks.org/problems/kadanes-algorithm/0).



#### Matrix:
- How to rorate a NxN image that is represented as a 2D array of integers in memory?
  - Follow-up: Can we do it without using extra memory?
  - Can we do it 1 pass?

#### Miscellaneous
- How do you check if two rectangles overlap with each other?

#### System design
- How do you design a vending machine?




![Python Data Structures](https://devopedia.org/images/article/41/4737.1513052765.jpg)


#### Referecnes
1. [MIT Advanced Algorithms](https://courses.csail.mit.edu/6.851/fall17/)
1. [https://simpleprogrammer.com/programming-interview-questions/](https://simpleprogrammer.com/programming-interview-questions/)
1. [https://www.geeksforgeeks.org/must-do-coding-questions-for-companies-like-amazon-microsoft-adobe/](https://www.geeksforgeeks.org/must-do-coding-questions-for-companies-like-amazon-microsoft-adobe/)
