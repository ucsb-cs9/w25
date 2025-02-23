---
layout: lab
num: lab07
ready: true
desc: "Fresh Tea Shipping System"
assigned: 2025-02-23 23:59:59.59-7
due: 2025-03-02 23:59:59.59-7
---

# Introduction

In this lab, we will utilize many concepts covered in the course so far including:

* Inheritance and Polymorphism
* Implementing and applying the Heap data structure as a priority queue
* Defining and raising Exceptions
* Testing your functionality with pytest

**Note: It is important that you try and start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the weekdays before the deadline if needed!**

You are the owner of a small tea business preparing to ship out orders of tea blends in time for the holidays. All placed orders are guaranteed to arrive before the holidays. All tea orders also have an associated shipping distance, and you want to ensure that customers from farther distances will receive their tea fresh and in time for the holidays. The goal for this lab is to write a program that will manage incoming tea orders by prioritizing orders with larger shipping distances. All tea orders will be managed by a MaxHeap where the next order to ship is the one that has the largest shipping distance compared to other orders.

In order to manage tea orders, you will design various Tea classes (`Tea`, `CustomTea`, and `SpecialtyTea` that utilizes inheritance / polymorphism), a `TeaOrder` class representing a collection of teas a customer wants to place in a single order, and an `OrderQueue` class that organizes the tea orders in a MaxHeap data structure.

You will also write pytests in a `testFile.py` illustrating your behavior works correctly. This lab writeup will provide some test cases for clarity, but the Gradescope autograder will run different tests shown here.

# Instructions

You will need to create six files:

* `Tea.py` - Defines a Tea class containing common attributes and functionality for all types of teas. For simplicity, this class will assume all teas have a size and price
* `CustomTea.py` - Defines a child class of Tea. This class should inherit all fields / methods from the Tea class, but also introduces a tea base (e.g. "black", "green", "oolong", etc.) and specific flavors a customer can order (represented as a list of strings)
* `SpecialtyTea.py` - Defines a child class of Tea. This class should also inherit all fields / methods from the Tea class. Specialty teas are defined by a name attribute and all have a set price depending on the tea size
* `TeaOrder.py` - Defines a class that is a collection of tea objects the customer wants to order. The total price for the order can be derived from each individual tea price. This class will also have a shipping distance. More details on this later in the writeup
* `OrderQueue.py` - Defines a MaxHeap to organize and process TeaOrders according to their expected time of pickup. You can adapt the Heap implementation shown in the textbook / lecture supporting the specifications in this lab
* `testFile.py` - This file will contain your pytest functions that tests the overall correctness of your class definitions

There will be no starter code for this assignment, but rather class descriptions and required methods are defined in the specification below.

You should organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import` / `from` technique shown in lecture.

# `Tea.py`

The `Tea.py` file will contain the definition of a Tea base class. We will define the Tea attributes as follows:

* `price` - `float` that represents the price of a Tea. Since the price will be defined by what type of Tea is ordered, we can simply create the price field and initialize it to `0.0`
* `size` - `str` that represents the size of the container for the Tea being ordered. For simplicity, we can have three valid sizes and and label these with "S" for small, "M" for medium, and "L" for large

You will write a constructor that allows the user to construct a Tea object by passing in values for the size. Your constructor should also create a price attribute and set it to `0.0`

* `__init__(self, size)`

Your Tea class definition should also support the "getter" and "setter" methods for the `price` and `size` attributes. Since this will be a base class for other tea types, anything we write here can be inherited by its child classes.

* `getPrice(self)`
* `setPrice(self, price)`
* `getSize(self)`
* `setSize(self, size)`

# `CustomTea.py` and `SpecialtyTea.py`

Tea objects can be categorized as two different types. Both of these types of teas inherit from the Tea class:

* `CustomTea` that allows the customers to customize a tea blend by choosing a base and add additional flavors of their choosing
* `SpecialtyTea` that has already been pre-configured with a fixed price based on its size

## `CustomTea.py`

Your `CustomTea` class definition will be defined in `CustomTea.py`. In addition to the fields inherited from the `Tea` class, a `CustomTea` object will contain the following additional attributes specific to a `CustomTea` item:

* `base` - `str` representing the tea base (e.g. "black", "green", "white", "oolong", etc)
* `flavors` - `list of str` representing additional flavors that are blended into the `CustomTea` item (e.g. "peach", "jasmine", "lavendar", "orange", etc)

The price of a `CustomTea` object is defined by two things:
* the size of the Tea
* the number of flavors this tea will have.

`CustomTea` objects will have the following fixed prices based on its size:

* Small (S): $10.00
* Medium (M): $15.00
* Large (L): $20.00

The size of the Tea also dictates the amount each additional flavor will cost based on the following criteria:

* Small (S): + $0.25 per additional flavor
* Medium (M): + $0.50 per additional flavor
* Large (L): + $0.75 per additional flavor

Since we now know what the price of a Tea should be based on the size, the `CustomTea` constructor should determine the base price of the Tea and update it appropriately based on the size. The `CustomTea` class will contain a constructor that takes in the size and base values for this object:
 * `__init__(self, size, base)`
    * The size attribute should be used to call our base class’ constructor. The `flavors` list is initialized to an empty list when constructed, and we can later add additional flavors

Other methods this class should support:
* `setBase(self, base)` - this method allows the customer to set a custom tea base (e.g. "black", "green", "white", "oolong", etc), represented as a str type
* `getBase(self)` - returns the base attribute string
* `addFlavor(self, flavor)` - this method will add to the flavors list and update its price appropriately. The flavor parameter is represented as a str type
* `getTeaDetails(self)` - this method will construct and return a string containing the details of the CustomTea object including the `size`, `base`, `flavors`, and `price` of the `CustomTea`.

An example (with escape characters shown for formatting) is given below. When constructing your string, please follow the EXACT format since this is what Gradescope will expect.

```python
cp1 = CustomTea("S", "Oolong")

assert cp1.getTeaDetails() == \
"CUSTOM TEA\n\
Size: S\n\
Base: Oolong\n\
Flavors:\n\
Price: $10.00\n"
```

An example (with escape characters shown for formatting) with a list of flavors is given below (note that each flavor will be indented with a tab):

```python
cp1 = CustomTea("L", "Green")
cp1.addFlavor("peach")
cp1.addFlavor("jasmine")

assert cp1.getTeaDetails() == \
"CUSTOM TEA\n\
Size: L\n\
Base: Green\n\
Flavors:\n\
\t+ peach\n\
\t+ jasmine\n\
Price: $21.50\n"
```

## `SpecialtyTea.py`

A `SpecialtyTea` class definition is defined in `SpecialtyTea.py`. Similar to a `CustomTea` object, the `SpecialtyTea` constructor will take in a size as well as the specific name (`str`) for the specialty tea item.

* `__init__(self, size, name)`

Similar to the `CustomTea` class, `SpecialtyTea` will use the size to set its price appropriately. The fixed price of a `SpecialtyTea` object is defined as follows:
* Small (S): $12.00
* Medium (M): $16.00
* Large (L): $20.00

Unlike `CustomTea` objects, `SpecialtyTea` objects do not have a base or a list of flavors associated with it, but do have a unique name that will be displayed when getting details for this tea blend. This class needs to support its own definition for:

* `getTeaDetails(self)` - this method will construct and return a string containing the details of the `SpecialtyTea` object including the size and name of the `SpecialtyTea` object. An example (with escape characters shown for formatting) is given below. Again, when constructing your string, please follow the EXACT format since this is what Gradescope will expect:

```python
sp1 = SpecialtyTea("S", "Earl Grey")

assert sp1.getTeaDetails() == \
"SPECIALTY TEA\n\
Size: S\n\
Name: Earl Grey\n\
Price: $12.00\n"
```

# `TeaOrder.py`

The `TeaOrder` class definition is defined in `TeaOrder.py`. This class will keep track of various teas items for a single order. The `TeaOrder` class will have the following attributes:

* `teas` - a Python List containing all the Tea objects (`CustomTea` and `SpecialtyTea` objects) that a single order contains. This can be initially set to an empty list
* `distance` - `int` representing the shipping distance in miles for the order

The constructor for a `TeaOrder` will take in the shipping distance and sets the `distance` attribute appropriately. Initially, the `TeaOrder` teas attribute will be empty when constructed, and tea objects can be added later:
* `__init__(self, distance)`

The ability to add Tea objects to the order, as well as a method to construct a string representing the order details will need to be implemented:

* `addTea(self, tea)` - adds a Tea object to the end of the `teas` Python List
* `getOrderDescription(self)` - constructs and returns a string containing the distance and price of this order, as well as all information for each tea item in the order. Since we’re storing various Tea objects in `teas`, we can utilize polymorphism and simply call the `getTeaDetails()` method on the Tea objects when constructing the string for our entire order, as well as `getPrice()` to compute the `TeaOrder` total price. The tea items in the order description will appear in the order they were inserted into `teas`

An example of the `getOrderDescription` return string format is given below (be sure to format your return string in the EXACT format since this is what Gradescope is expecting):

```python
ct1 = CustomTea("S", "Black")
ct1.addFlavor("rose")
ct1.addFlavor("cardamom")
st1 = SpecialtyTea("M", "Matcha")
order = TeaOrder(400) #shipping distance: 400 miles 
order.addTea(ct1)
order.addTea(st1)

assert order.getOrderDescription() == \
"******\n\
Shipping Distance: 400 miles\n\
CUSTOM TEA\n\
Size: S\n\
Base: Black\n\
Flavors:\n\
\t+ rose\n\
\t+ cardamom\n\
Price: $10.50\n\
\n\
----\n\
SPECIALTY TEA\n\
Size: M\n\
Name: Matcha\n\
Price: $16.00\n\
\n\
----\n\
TOTAL ORDER PRICE: $26.50\n\
******\n"
```

# `OrderQueue.py`

The `OrderQueue` class will be defined in `OrderQueue.py`. This priority queue is implemented as a MaxHeap data structure. The `OrderQueue` will manage `TeaOrder` objects based on their distance attribute.

The `OrderQueue` constructor (`__init__(self)`) will simply initialize the underlying Python List representing the MaxHeap.

Since it’s possible to attempt to remove from an empty `OrderQueue`, we will create and raise an exception when this is done. You will define a `QueueEmptyException` class in `OrderQueue.py` that doesn’t do anything except define an Exception object to raise when this happens (recall, we did do an example of defining basic Exception class types and raising this exception in lecture - you can refer to [lect06](https://ucsb-cs9.github.io/w25/lectures/lect06/) notes).

In addition to the construction of the MaxHeap in this class, two methods are required to be implemented:
* `addOrder(self, teaOrder)` - a `TeaOrder` object will be stored in the MaxHeap prioritized by its distance attribute (higher value means higher priority)
* `processNextOrder(self)` - removes the root node from the MaxHeap (and restructures the MaxHeap), and returns a string containing the `TeaOrder` description that was just removed

The automated tests will create various tea orders with different distance attributes. It will then call `processNextOrder` one at a time and check the removed `TeaOrder` is in the right priority by checking their expected `getOrderDescription` string. You should write similar tests to confirm the MaxHeap state is in the correct order.

# `testFile.py`

This file should test all of your classes using `pytest`. Think of various scenarios and edge cases when creating your own tests according to the given descriptions. You should test every class’ method functionality (except for getters / setters). Even though Gradescope will not use this file when running automated tests (there are separate tests defined for this), it is important to provide this file with various test cases (testing is important!!).

Of course, feel free to reach out / post questions on Piazza as they come up!

# Submission

Once you’re done with writing your class definitions and tests, submit the following files to Gradescope’s `Lab07` assignment:
* `Tea.py`
* `CustomTea.py`
* `SpecialtyTea.py`
* `TeaOrder.py`
* `OrderQueue.py`
* `testFile.py`

There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

If the tests don’t pass, you may get some error message that may or may not be obvious at this point. Don’t worry - take a minute to think about what may have caused the error and try writing various test scenarios to see if your code is behaving correctly according to the specifications. If you’re still not sure why you’re getting the error, feel free to ask your TAs / Learning Assistants.

<sup>* Lab07 created by Emily Tian and adapted / updated by Richert Wang (W25)</sup>
