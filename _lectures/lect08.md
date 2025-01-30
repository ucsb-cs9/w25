---
num: "Lecture 8"
desc: "Binary Search, Midterm Guide"
ready: true
lecture_date: 2025-01-30 11:00:00.00-7:00
---

Recorded Lecture: [1_30_25](https://drive.google.com/file/d/178xAnvw4rX32qdP-kvDa83WIWlSz9nq-/view?usp=drive_link)

# Recall

* Recall our benchmarking algorithm when inserting elements to the front of a Python list:

```
def f1(n):
	l = []
	for i in range(n):
		l.insert(0,i)
	return
```

* Since we're inserting elements at the front of the Python list, we need to move the existing elements over by one in order to make room for them.
	* So the first insertion is cheap and takes one step
	* The 2nd insertion needs to move the first element over by one in order to make room for the 2nd element
	* The 3rd insertion needs to move the first and second element over
	* and so on...
* We can say the number of steps this for loop has is:
	* 1 + 2 + 3 + ... + n
	* And this simplifies to n(n + 1) / 2
	* Therefore, `f1` is **O(n<sup>2</sup>)**

# Binary Search Algorithm

* **Binary Search** is a useful algorithm to search for an item in a collection **IF THE ITEMS ARE IN SORTED ORDER**
	* Since the collection is in sorted order, we can check the middle to see if the item we're looking for is there
		* If the middle element is the one we're looking for, then great!
		* If the item we're looking for is < the middle element, then we don't have to search the right side of the collection
		* If the item we're looking for is > the middle element, then we don't have to search the left side of the collection
	* Since each comparison is eliminating half the search space, this algorithm has a logarithmic property
		* And this algorithm performs in **O(log n)** time

## Let's do some TDD and write tests before we write the function!

```
def test_binarySearchNormal():
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 5) == True
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], -1) == False
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 11) == False
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 1) == True
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 10) == True

def test_binarSearchDuplicates():
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 0) == True
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 2) == False
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 4) == True
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 2) == False

def test_binarySearchEmptyList():
	assert binarySearch([], 0) == False

def test_binarySearchSameValues():
	assert binarySearch([5,5,5,5,5,5,5,5,5,5,5], 5) == True
	assert binarySearch([5,5,5,5,5,5,5,5,5,5,5], 0) == False
```

## Binary Search Implementation (iterative)

```
def binarySearch(intList, item):
	first = 0
	last = len(intList) - 1
	found = False

	while first <= last and not found:
		mid = (first + last) // 2
		if intList[mid] == item:
			found = True
		else:
			if item < intList[mid]:
				last = mid - 1
			else:
				first = mid + 1
	return found
```

## Binary Search (recursive)

```
def binarySearch(intList, item):
	if len(intList) == 0: # base case
		return False

	mid = len(intList) // 2
	if intList[mid] == item:
		return True
	elif item < intList[mid]:
		return binarySearch(intList[0:mid], item)
	else:
		return binarySearch(intList[mid+1:], item)
```

# Midterm Guide

```
Time:
* In-person (BRDA 1610), Thursday 2/6 11am - 12:15pm PST

Logistics:
* Bring a writing utensil and student ID
* Please write legibly (if we can't read your answer, we can't grade it)
* When leaving the room, you must hand in your exam and present your ID
* Closed book (no notes / book / electronic devices)
* TAs and I will be proctoring the exam. We can answer any CLARIFYING questions

Possible Types of Questions:
* True / False (if False, briefly state why (1 sentence))
* Short Answer - Briefly define / state / describe / ...
* Given some code, write the output / state the O-notation
* Write code satisfying a specification
* Given some partial algorithm, fill in the blank to make it work
* Draw diagrams

Midterm Topics:

Will cover material from the beginning of the class up until the end of week 4's (1/30) lecture

Python Basics 
- Types
    * I don't expect students to know ALL Python basics, but do expect students to know
    the ones we explicitly covered in lectures and/or assignments
    * int, float, boolean, strings, lists, sets, dictionaries, ...
    * conversion functions (int(), float(), str(), ...)
    * mutable vs. immutable
- Relational / logical operators (==, <, >, <=, >=, and, or, not, ...)
- Python Lists
    * Supporting methods (count, pop, append, insert, ...)
    * List / string slicing [:]
- Understanding under-the-hood functionality of lists and dictionaries
- Strings
    * Supporting methods (replace, split, find, ...)
- Function definitions
- Control Structures (if, else, elif, for, while, ...)

Python Errors / Exceptions
- Runtime vs. Syntax errors
- Exception types (NameError, TypeError, ZeroDivisionError, ...)
- Exception handling
    * try / except / raise
    * Flow of execution
    * Passing exceptions to function / method caller(s) when not handled in try / except
    * Multiple except blocks
    * Handling Exceptions in an Inheritance hierarchy

Object Oriented Programming
- Defining classes and methods
- constructors (defining / initializing default state)
- Shallow vs. Deep equality (overloading __eq__ method)
- Overloading operators (__str__, __add__, __le__, ...)
- Inheritance
    * know method lookup for inheritance hierarchy
    * implementation for inherited fields / methods
    * overriding inherited methods
    * calling super / base class(es) methods

Testing
- Test Driven Development (TDD)
- pytest

Algorithm Analysis and O-notation
- Know how to derive O-notation for snippets of code

Recursion
- Implementation of recursive functions and O-notation analysis
- Understanding how recursive algorithms are managed by the call stack
- Draw call stack given a recursive function

Binary Search
- Search for items in a SORTED list
- Know the implementations (as covered in textbook / lecture) and O-notation

```
