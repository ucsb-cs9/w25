---
layout: lab
num: lab05
ready: true
desc: "Ordered Linked Lists"
assigned: 2025-02-09 23:59:59.59-7
due: 2025-02-16 23:59:59.59-7
---

In this lab, you'll have the opportunity to practice:

* Defining classes in Python
* Overloading an operator in a Python class
* Implementing an Ordered Linked List
* Testing your functionality with pytest

**Note: It is important that you start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the week before the deadline if needed!**

You will write a program that will organize Movie objects into a Movie Collection. The Movie Collection will be implemented as an Ordered Linked List with a head reference. The implementation in this lab will be different than an Unordered Linked List since you will need to organize the nodes by the Movie's director name (last name, first name in lexicographical / alphabetical order). In the event of a tie (several movies are directed by the same person), the year the movie was released will be used to determine the Movie object's place in the Ordered Linked List. If the director and year are the same, then the Movie's name (lexicographical / alphabetical order) will be used to determine the Movie object's place in the Ordered Linked List.

This lab will require you to define classes for a `Movie`, `MovieCollection`, and a `MovieCollectionNode`, as well as writing your own unit tests to verify the correctness of your implementation.

# Instructions

You will need to create four files:
* `Movie.py` - file containing a class definition for a Movie object
* `MovieCollection.py` - file containing a class definition for a collection of Movies that will be implemented as an Ordered Linked List
* `MovieCollectionNode.py` - file containing a class definition for a Node in the MovieCollection
* `testFile.py` - file containing pytest functions testing the overall correctness of your class definitions

There will be no starter code for this assignment, but rather the class descriptions and required methods are defined in the specification below.

You should organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import / from` technique shown in lecture.

# Movie.py class

The `Movie.py` file will contain the definition of a Movie. We will define the Movie attributes as follows:

* `movieName` - str that represents the name of the Movie **in all uppercase characters**.
* `director` - str that represents the director of the Movie **in all uppercase characters**. This will be in a LastName, FirstName format. You may assume this will always be the case.
* `year` - int that represents the year the Movie was released
* `rating` - float that represents the rating of this movie by the owner of the collection. 

You should write a constructor that allows the user to construct a movie object by passing in values for `movieName`, `director`, `year`, and `rating`. Your constructor should set the `rating` to `None` by default if a value is not specified. You can assume that the `movieName` and `director` will always be of type `str` and the `year` will always be of type `int` when your code is run.
* `__init__(self, movieName, director, year, rating=None)`

In addition to your constructor, your class definition should also support "getter" methods that can receive the state of the Movie object:

* `getMovieName(self)`
* `getDirector(self)`
* `getYear(self)`
* `getRating(self)`

You will implement the method

* `getMovieDetails(self)`

that returns a `str` with all of the Movie attributes. The string should contain all attributes in the following EXACT format. The director's name should be displayed as FirstName LastName according to example output below. (**Note: There is no `\n` character at the end of this string**):

```python
m1 = Movie("The Godfather", "Darabont, Frank", 1972, 9.4)
m2 = Movie("2001: A Space Odyssey", "Kubric, Stanley", 1968)
print(m1.getMovieDetails())
print(m2.getMovieDetails())
```

<b>Output</b>

```
THE GODFATHER directed by FRANK DARABONT (1972), Rating: 9.4
2001: A SPACE ODYSSEY directed by STANLEY KUBRIC (1968), Rating: None
```

* Lastly, your `Movie` class will overload the `>` operator (`__gt__`). This will be used when finding the proper position of a Movie in the Ordered Linked List using the specification above. We can compare movies using the `>` operator while walking down the list and checking if the inserted movie is `>` than a specific Movie in the Ordered Linked List.

We reviewed operator overloading in class and the textbook does discuss overloading Python operators. You can also refer to this reference on overloading various operators as well:

[https://www.geeksforgeeks.org/operator-overloading-in-python/](https://www.geeksforgeeks.org/operator-overloading-in-python/)

# MovieCollection.py and MovieCollectionNode.py

The `MovieCollectionNode.py` file will define the `MovieCollectionNode` class. This will be similar to the Linked List Node implementation done in lecture. 

You will need to write the following methods for the `MovieCollectionNode` class:

* `__init__(self, data)` - constructor that will assign the parameter data (a Movie object) as part of this node. Each node will also have a next reference associated with it. When constructing a `MovieCollectionNode`, the next reference should be `None` by default
* `getData(self)` - getter method that returns the data (Movie object)
* `getNext(self)` - getter method that returns the next `MovieCollectionNode` in the Linked List structure
* `setData(self, newData)` - updates the Node's data with the parameter `newData`
* `setNext(self, newNode)` - updates the Node's `next` reference with the newNode parameter (another `MovieCollectionNode`)

The `MovieCollection.py` file will contain the definition of a collection of Movie objects stored in an Ordered Linked List. Your class implementation **MUST** have an attribute called `head` that refers to the node at the front of the linked list or set this value to `None` if the linked list is empty. This allows the autograder to properly test your code. The MovieCollection will manage an Ordered Linked List containing `MovieCollectionNode` objects. The `MovieCollection` class will be responsible for maintaining the overall structure of the Ordered Linked List. Your `MovieCollection` class will need to support the following methods:

* `__init__(self)` - constructor that will have a `head` reference that refers the first node in the Ordered Linked List. This should be set to `None` when constructing an empty `MovieCollection` object.
* `isEmpty(self)` - method that returns `True` (boolean) if the MovieCollection is empty, and returns `False` otherwise
* `getNumberOfMovies(self)` - method that returns the total number of Movies (int) in the MovieCollection
* `insertMovie(self, movie)` - method that inserts a Movie object in the appropriate place. As mentioned before, all Movies in the MovieCollection are ordered based on 1) the Movie's director ("Last Name, First Name" in alphabetical / lexicographical order), then 2) the year the Movie was released, and then 3) the Movie's name (alphabetical / lexicographical order). Here is where we can utilize the Movie's `>` operator. You'll need to do some logic to replace the references for the MovieCollectionNodes when inserting the Movie in the appropriate place. (**Hint: You can also overload the `__gt__`, `__lt__`, and `__eq__` operators in the Movie class to make this method easier to implement.**)
* `getAllMoviesInCollection(self)` - method that returns a `str` containing the details of all Movies in the MovieCollection. **Note that each movie will be in its own line (each line ending with a `\n` character)**. An example (with correct order) is shown below:

```python
m0 = Movie("Casablanca", "Curtiz, Michael", 1942, 8.8)
m1 = Movie("2001: A Space Odyssey", "Kubric, Stanley", 1968, 9.2)
m2 = Movie("The Matrix", "Wachowski, Lana", 1999, 9.1)
m3 = Movie("Modern Times", "Chaplin, Charlie", 1936, 8.9)
m4 = Movie("City Lights", "Chaplin, Charlie", 1931, 9.0)

mc = MovieCollection()
mc.insertMovie(m0)
mc.insertMovie(m1)
mc.insertMovie(m2)
mc.insertMovie(m3)
mc.insertMovie(m4)
print(bc.getAllMoviesInCollection())
```

<b> Output: </b>

```
CITY LIGHTS directed by CHARLIE CHAPLIN (1931), Rating: 9.0
MODERN TIMES directed by CHARLIE CHAPLIN (1936), Rating: 8.9
CASABLANCA directed by MICHAEL CURTIZ (1942), Rating: 8.8
2001: A SPACE ODYSSEY directed by STANLEY KUBRIC (1968), Rating: 9.2
THE MATRIX directed by LANA WACHOWSKI (1999), Rating: 9.1

```

* `getMoviesByDirector(self, director)` - method that returns a str containing all of the Movie details by a specified director in the order specified for this lab assignment. This method should return the empty string `""` if there are no movies directed by the `director` parameter. **Note that each movie will be in its own line (each line ending with a `\n` character)**. **Also note that the input parameter (`director`) may be in a different case than the case of the director that the movie was constructed with** An example (with correct order) is shown below:

```python
m0 = Movie("Casablanca", "Curtiz, Michael", 1942, 8.8)
m1 = Movie("2001: A Space Odyssey", "Kubric, Stanley", 1968, 9.2)
m2 = Movie("The Matrix", "Wachowski, Lana", 1999, 9.1)
m3 = Movie("Work", "Chaplin, Charlie", 1915, 6.2)
m4 = Movie("Modern Times", "Chaplin, Charlie", 1936, 8.9)
m5 = Movie("City Lights", "Chaplin, Charlie", 1931, 9.0)
m6 = Movie("The Champion", "Chaplin, Charlie", 1915, 6.7)

mc = MovieCollection()
mc.insertMovie(m0)
mc.insertMovie(m1)
mc.insertMovie(m2)
mc.insertMovie(m3)
mc.insertMovie(m4)
mc.insertMovie(m5)
mc.insertMovie(m6)
print(mc.getMoviesByDirector("Chaplin, Charlie"))
```

<b> Output: </b>

```
THE CHAMPION directed by CHARLIE CHAPLIN (1915), Rating: 6.7
WORK directed by CHARLIE CHAPLIN (1915), Rating: 6.2
CITY LIGHTS directed by CHARLIE CHAPLIN (1931), Rating: 9.0
MODERN TIMES directed by CHARLIE CHAPLIN (1936), Rating: 8.9

```

* `removeDirector(self, director)` - method that removes all Movies directed by a specified director. Since this is an Ordered Linked List, all Movies directed by a specific director should be located right next to each other in the Movies Collection assuming your `insertMovie` method has been implemented correctly. **Note:** the input parameter (`director`) may be in a different case than the case of the director that the movie was constructed with - your solution must account for these situations.
* `avgDirectorRating(self, director)` - method that returns a `float` representing the average rating of Movies directed by a specified director. Similarly, all Movies directed by a specific director should be located right next to each other in the Movies Collection. You should round the average of rating to two decimals. Any movies with a rating of `None` should be ignored. If a director has no ratings, then this function should return `None`. For example:

```python
m0 = Movie("Casablanca", "Curtiz, Michael", 1942, 8.8)
m1 = Movie("Modern Times", "Chaplin, Charlie", 1936, 8.9)
m2 = Movie("The Matrix", "Wachowski, Lana", 1999, 9.1)
m3 = Movie("City Lights", "Chaplin, Charlie", 1931, 9.0)

mc = MovieCollection()
mc.insertMovie(m0)
mc.insertMovie(m1)
mc.insertMovie(m2)
mc.insertMovie(m3)
print(mc.avgDirectorRating("Chaplin, Charlie"))
print(mc.avgDirectorRating("Curtiz, Michael"))
```

<b> Output: </b>

```
8.95
8.8
```

* `recursiveSearchMovie(self, movieName, movieNode)` - method that searches the `MovieCollection` for a specific Movie name passed in the method. **Note: this method must be done recursively.** This method will return `True` if a Movie in the MovieCollection has the same name as the parameter `movieName` (str), and return `False` otherwise.
	* Since this solution is recursive, this method will take in a reference to a `MovieCollectionNode` object (`movieNode`) in the `MovieCollection` that needs to be searched.
	* The initial call to `recursiveSearchMovie` would pass in the head of the `MovieCollection` since that's the starting point to search through the entire `MovieCollection`. For example:

```python
m0 = Movie("Casablanca", "Curtiz, Michael", 1942, 8.8)
m1 = Movie("2001: A Space Odyssey", "Kubric, Stanley", 1968, 9.2)
m2 = Movie("Modern Times", "Chaplin, Charlie", 1936, 8.9)
m3 = Movie("City Lights", "Chaplin, Charlie", 1931, 9.8)

mc = MovieCollection()
mc.insertMovie(m0)
mc.insertMovie(m1)
mc.insertMovie(m2)
mc.insertMovie(m3)
assert mc.recursiveSearchMovie("Modern times", mc.head) == True
assert mc.recursiveSearchMovie("The Matrix", mc.head) == False
```

	* You can then recursively search through the `MovieCollection` sub parts by recursively referring to the next `MovieCollectionNode` in `MovieCollection` that needs to be searched if the Movie in `movieNode` does not have the movie name we're looking for.
	* **Note:** the input parameter `movieName` may be in a different case than the case of the movie name that the Movie was constructed with - your solution must account for these situations (see assert statement above).

## testFile.py pytest

This file should import your `Movie`, `MovieCollection`, and `MovieCollectionNode` classes so you can write unit tests using pytest to test your functionality is correct. Think of various scenarios and edge cases when testing your code. Write your tests first in order to check the correctness of the `Movie`, `MovieCollection` and `MovieCollectionNode` methods. Gradescope requires `testfile.py` to be submitted before running any autograded tests. You should write at least one test for each method in each of these classes (but more tests can help you debug various cases!).

## Submission

Once you're done with writing your class definitions and tests, submit your `Movie.py` and `MovieCollection.py`, `MovieCollectionNode.py`, and `testFile.py` files to the `Lab05` assignment on Gradescope. Remember to remove any `print` statements in your code since this may confuse the autograder. There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

If the tests don't pass, you may get some error message that may or may not be obvious. Don't worry - if the tests didn't pass, take a minute to think about what may have caused the error, and try writing more comprehensive tests for various cases. If your tests didn't pass and you're still not sure why you're getting the error, feel free to ask your TAs or Learning Assistants.

<sup>* Lab05 created by Sanjay Chandrasekaran and Qiucheng Wu, and adapted / updated by Richert Wang (W25)</sup>