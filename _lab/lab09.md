---
layout: lab
num: lab09
ready: true
desc: "BST Course Catalog - Part 2"
assigned: 2025-03-09 23:59:59.59-7
due: 2025-03-16 23:59:59.59-7
---

# Introduction

In this lab, you will build upon your Lab08 code by adding two new methods to the `CourseCatalog` class: `removeSection` and `removeCourse`.

* `removeSection` - removes a section from a course node in the Binary Search Tree
* `removeCourse` - removes a course node from the Binary Search Tree

You will also write a test file to verify your implementation. Note: while this lab writeup provides sample test cases for reference, the Gradescope autograder will use different, more complex test cases.

**Note: It is important that you start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the weekdays before the deadline if needed!**

# Instructions

The lab specifications in Lab08 apply to this lab, but this lab will specifically focus on removal functionality (courses and sections) in the Course Catalog Binary Search Tree. Lab08 tests are included in Lab09's Gradescope tests in case you were unable to finish Lab08 by the deadline and would like to check your Lab08 functionality in this lab.

## CourseCatalog.py

The `CourseCatalog.py` file will contain the definition of the `CourseCatalog` class, which is a Binary Search Tree that manages all `CourseCatalogNode` objects keyed by `department` and `courseId`. For additional details about the existing requirements, please refer to the Lab08 instructions.

In addition to the previously created methods, the following new methods are required to be implemented:

* Methods:

    * `removeCourse(self, department, courseId)` - returns a boolean value. Remove a course node identified by `department` and `courseId` from the Binary Search Tree. If the course does not exist, return `False` and do nothing. Otherwise, remove the course node and return `True`. **Note: The `department` parameter may not necessarily be in uppercase**
    * `removeSection(self, department, courseId, section)` - returns a boolean value. Remove a section (containing the same section `Event` fields) from a course node in the Binary Search Tree. If the course identified by `department` and `courseId` does not exist in the Course Catalog, or if the course exists but the section does not exist, return `False` and do nothing. Otherwise, remove the section from the Course Catalog Node's sections list (`sections`) if it exists, and return `True`. You can assume there are no duplicate sections in a course node's sections list. **Note: The `department` parameter may not necessarily be in uppercase**

Note: When removing a course node with two children (case 3), use the technique of replacing it with its successor as discussed in the lecture and textbook. You may find it useful to implement a getSuccessor helper method to find the successor of a node, following the approach outlined in the textbook.

**Example:**

```python
cc = CourseCatalog()

# add a new course: cmpsc 9
lecture = Event("TR", (1530, 1645), "td-w 1701")
section1 = Event("W", (1400, 1450), "north hall 1109")
section2 = Event("W", (1500, 1550), "north hall 1109")
section3 = Event("W", (1600, 1650), "north hall 1109")
section4 = Event("W", (1700, 1750), "girvetz hall 1112")
sections = [section1, section2, section3, section4]
assert True == cc.addCourse("cmpsc", 9, "intermediate python", lecture, sections)

# add a new course: art 10
lecture = Event("TR", (1300, 1550), "arts 2628")
sections = []
assert True == cc.addCourse("art", 10, "introduction to painting", lecture, sections)

'''
                                                                   root
                                             ------------------------------------------------
                                            | CMPSC 9: INTERMEDIATE PYTHON                   |
                                            |  * Lecture: TR 15:30 - 16:45, TD-W 1701        |
                                            |  + Section: W 14:00 - 14:50, NORTH HALL 1109   |
                                            |  + Section: W 15:00 - 15:50, NORTH HALL 1109   |
                                            |  + Section: W 16:00 - 16:50, NORTH HALL 1109   |
                                            |  + Section: W 17:00 - 17:50, GIRVETZ HALL 1112 |
                                             ------------------------------------------------
                                            /
     -----------------------------------------
    | ART 10: INTRODUCTION TO PAINTING        |
    |  * Lecture: TR 13:00 - 15:50, ARTS 2628 |
     -----------------------------------------
'''

print("----- in-order traversal -----")
print(cc.inOrder())

print("----- pre-order traversal -----")
print(cc.preOrder())

# remove a section from cmpsc 9
section = Event("W", (1500, 1550), "north hall 1109")
assert True == cc.removeSection("cmpsc", 9, section)

'''
                                                                   root
                                             ------------------------------------------------
                                            | CMPSC 9: INTERMEDIATE PYTHON                   |
                                            |  * Lecture: TR 15:30 - 16:45, TD-W 1701        |
                                            |  + Section: W 14:00 - 14:50, NORTH HALL 1109   |
                                            |  + Section: W 16:00 - 16:50, NORTH HALL 1109   |
                                            |  + Section: W 17:00 - 17:50, GIRVETZ HALL 1112 |
                                             ------------------------------------------------
                                            /
     -----------------------------------------
    | ART 10: INTRODUCTION TO PAINTING        |
    |  * Lecture: TR 13:00 - 15:50, ARTS 2628 |
     -----------------------------------------
'''

print("----- in-order traversal -----")
print(cc.inOrder())

print("----- pre-order traversal -----")
print(cc.preOrder())

# remove cmpsc 9
assert True == cc.removeCourse("cmpsc", 9)

'''
                       root
     -----------------------------------------
    | ART 10: INTRODUCTION TO PAINTING        |
    |  * Lecture: TR 13:00 - 15:50, ARTS 2628 |
     -----------------------------------------
'''

print("----- in-order traversal -----")
print(cc.inOrder())

print("----- pre-order traversal -----")
print(cc.preOrder())
```

**Output:**
```
----- in-order traversal -----
ART 10: INTRODUCTION TO PAINTING
 * Lecture: TR 13:00 - 15:50, ARTS 2628
CMPSC 9: INTERMEDIATE PYTHON
 * Lecture: TR 15:30 - 16:45, TD-W 1701
 + Section: W 14:00 - 14:50, NORTH HALL 1109
 + Section: W 15:00 - 15:50, NORTH HALL 1109
 + Section: W 16:00 - 16:50, NORTH HALL 1109
 + Section: W 17:00 - 17:50, GIRVETZ HALL 1112

----- pre-order traversal -----
CMPSC 9: INTERMEDIATE PYTHON
 * Lecture: TR 15:30 - 16:45, TD-W 1701
 + Section: W 14:00 - 14:50, NORTH HALL 1109
 + Section: W 15:00 - 15:50, NORTH HALL 1109
 + Section: W 16:00 - 16:50, NORTH HALL 1109
 + Section: W 17:00 - 17:50, GIRVETZ HALL 1112
ART 10: INTRODUCTION TO PAINTING
 * Lecture: TR 13:00 - 15:50, ARTS 2628

----- in-order traversal -----
ART 10: INTRODUCTION TO PAINTING
 * Lecture: TR 13:00 - 15:50, ARTS 2628
CMPSC 9: INTERMEDIATE PYTHON
 * Lecture: TR 15:30 - 16:45, TD-W 1701
 + Section: W 14:00 - 14:50, NORTH HALL 1109
 + Section: W 16:00 - 16:50, NORTH HALL 1109
 + Section: W 17:00 - 17:50, GIRVETZ HALL 1112

----- pre-order traversal -----
CMPSC 9: INTERMEDIATE PYTHON
 * Lecture: TR 15:30 - 16:45, TD-W 1701
 + Section: W 14:00 - 14:50, NORTH HALL 1109
 + Section: W 16:00 - 16:50, NORTH HALL 1109
 + Section: W 17:00 - 17:50, GIRVETZ HALL 1112
ART 10: INTRODUCTION TO PAINTING
 * Lecture: TR 13:00 - 15:50, ARTS 2628

----- in-order traversal -----
ART 10: INTRODUCTION TO PAINTING
 * Lecture: TR 13:00 - 15:50, ARTS 2628

----- pre-order traversal -----
ART 10: INTRODUCTION TO PAINTING
 * Lecture: TR 13:00 - 15:50, ARTS 2628

```

## testFile.py

This file should contain tests for the new methods `removeSection` and `removeCourse`, written using `pytest`. For `removeCourse`, the tests should cover cases 1, 2, and 3 (as discussed in lecture) at a minimum.

While Gradescope will not execute this file during automated testing, you can run it locally to debug and verify that your code functions correctly. Please remember, testing is important!

# Submission

Once you have completed the new methods and tests, submit the following files to Gradescope’s Lab09 assignment:

* `Event.py`
* `CourseCatalogNode.py`
* `CourseCatalog.py`
* `testFile.py`

Gradescope will run a variety of unit tests to verify that your code works correctly according to the specifications provided in this lab.

If the tests don’t pass, you may get some error messages that may not be immediately clear. Don’t worry — take a moment to consider what might have caused the error, and double-check the instructions to ensure your code is following the specifications precisely. Consider writing additional test cases for various scenarios / edge cases to help you debug any issues. If you’re still unsure why the error is occurring, feel free to ask your TAs or Learning Assistants for assistance.

<sup>* Lab09 created by Jiajun Wang and Ysabel Chen, and adapted / updated by Richert Wang (W25)</sup>