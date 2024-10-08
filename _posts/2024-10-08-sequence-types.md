---
title: 'Dunder Methods'
date: 2024-10-04
permalink: /posts/2024/10/SequenceTypes/
tags:
  - Python
  - Books
---


#### Built-in Sequences in Python
Python provides several built-in sequence types that are implemented in **C**, which are divided into two categories based on how they store their contents: **Container sequences** and **Flat sequences**.

#### 1. **Container Sequences**

**Container sequences** can hold items of **different types**, including nested containers. They are called "containers" because they store references to the objects they hold rather than the objects themselves. This means that the items inside container sequences can be of any type, including other sequences.

- **List** (`list`)
- **Tuple** (`tuple`)
- **Deque** (`collections.deque`)

##### Example:
```python
my_list = [1, "apple", [2, 3], {'key': 'value'}]  # Different types of objects in one list
```
Each item in this `list` is a reference to a Python object, whether it’s an integer, a string, another list, or a dictionary.

#### 2. **Flat Sequences**

**Flat sequences** store items of **one specific, simple type**. They are more **compact** than container sequences because they store the actual values in contiguous memory. However, they are limited to storing primitive machine types like bytes, integers, and floats. 

- **String** (`str`)
- **Bytes** (`bytes`)
- **Array** (`array.array`)

##### Example:
```python
import array
my_array = array.array('d', [1.0, 2.0, 3.0])  # Array of floats ('d' stands for double)
```
In this case, the array holds machine-level **floating-point values** directly in memory, rather than references to Python objects.

### Key Difference: How Data Is Stored

- **Container sequences**: Hold **references** to objects. Each object has its own memory location, and Python needs to store both the reference and the object’s internal structure (**metadata**).
- **Flat sequences**: Hold **values directly** in memory. They are more memory-efficient because they don't store Python objects, only the actual raw data in contiguous memory.

![[Pasted image 20241007094559.png]]
*Left: Container Sequence
Right: Flat sequence*

---
### In-Memory Representation

The **gray cells** in the illustration refer to the **in-memory header** of each Python object. This header contains metadata about the object, such as its type, reference count, and other internal details that Python uses to manage the object.

- In **container sequences**, each item is a **separate Python object**, with its own in-memory header and metadata.
- In **flat sequences**, the sequence holds a block of memory that directly contains the raw data, without separate Python objects for each item.

For example:
- In a **list**, each element is a reference to a separate Python object (even for integers).
- In an **array**, all elements are stored in a single block of memory, making it more efficient in terms of both memory and performance.

### Grouping Sequences by Mutability

Another way to classify sequences in Python is by their **mutability**—whether their contents can be modified after creation.
#### 1. **Mutable Sequences**
Mutable sequences allow you to change, add, or remove items after the sequence is created.

- **List** (`list`)
- **Bytearray** (`bytearray`)
- **Array** (`array.array`)
- **Deque** (`collections.deque`)

##### Example:
```python
my_list = [1, 2, 3]
my_list[0] = 10  # You can modify items in a mutable sequence
```

#### 2. **Immutable Sequences**
Immutable sequences do not allow any modification once they are created. Any attempt to change the contents of an immutable sequence will result in an error.

- **Tuple** (`tuple`)
- **String** (`str`)
- **Bytes** (`bytes`)

##### Example:
```python
my_tuple = (1, 2, 3)
my_tuple[0] = 10  # This will raise a TypeError because tuples are immutable
```

------
### List Comprehensions and Generator Expressions
#### [[List Comprehensions]] and Readability 

List comprehensions in Python provide a concise and readable way to create new lists by applying an expression to each element in an existing iterable. The general form is:

```python
newlist = [_expression_ for _item_ in _iterable_ if _condition_ == True]
```

This one-liner syntax makes list comprehensions much more readable and shorter than the traditional approach of using `for` loops. For example:

```python
symbols = '$¢£¥€¤'
codes = [ord(symbol) for symbol in symbols]
```
This creates a list called `codes` with the Unicode code points of each symbol in the `symbols` string.

##### Cartesian Products with List Comprehensions
You can also use list comprehensions to generate combinations of values, like creating cartesian products (all combinations of elements from two lists).

```python
colors = ['black', 'white']
sizes = ['S', 'M', 'L']
tshirts = [(color, size) for color in colors for size in sizes]
```
This results in:
```python
[('black', 'S'), ('black', 'M'), ('black', 'L'),
 ('white', 'S'), ('white', 'M'), ('white', 'L')]
```

#### **List Comprehensions** vs. `map` and `filter`
List comprehensions can often be used as alternatives to the `map()` and `filter()` functions. Let’s break them down:

- **`map()`** applies a function to every item in an iterable (like a list) and returns an iterator.
    - Example: 
```python
symbols = '$¢£¥€¤'
mapped = list(map(ord, symbols))  # Applies the `ord` function to each element in symbols
    ```

- **`filter()`** takes a function and an iterable, returning only the items for which the function returns `True`.
    - Example: 
```python
beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))
```

In this example, the `map()` function applies `ord` to each symbol, and `filter()` only keeps those with a Unicode code point greater than 127. 

##### **Performance Comparison**
- **List comprehensions** tend to be faster because they are optimized in Python’s C implementation.
- **`map()` and `filter()`** are useful when you want to apply existing functions to sequences or when using higher-order functions like `lambda`.

When to use:
- **List comprehensions** are generally preferred for readability and performance, especially for simple transformations.
- **`map()` and `filter()`** can be useful when you have an existing function and want to apply it across an iterable without rewriting the logic in a list comprehension.

#### **Generator Expressions**
While list comprehensions create lists, **generator expressions** (or **genexps**) create **generators**, which generate values **one at a time**, thus saving memory when dealing with large datasets. 

The syntax for a generator expression is similar to a list comprehension but uses parentheses instead of square brackets:

```python
symbols = '$¢£¥€¤'
genexpr = (ord(symbol) for symbol in symbols)
```
This doesn't create a list but instead yields the values on demand. For instance, you could convert it into a tuple:

```python
tuple(ord(symbol) for symbol in symbols)
```

##### **Why Use Generator Expressions?**
- **Memory Efficiency**: Generator expressions are more memory-efficient than list comprehensions because they don't store the entire result in memory. Instead, they yield items one by one, making them ideal for iterating over large data streams.
- **Use Case**: If you are only interested in processing the elements one at a time (e.g., in a `for` loop), a generator expression is a better choice.

#### Example of Memory Efficiency:
```python
large_gen = (i * i for i in range(10**6))  # Does not store all values in memory at once
```

### Tuples

#### **Tuples as Records**
Tuples are often used as **records** to store related but heterogeneous data, where each position in the tuple represents a different field. The structure and order of the items are crucial, as the position of each element gives it meaning.

Example:
```python
lax_coordinates = (33.9425, -118.408056)  # Longitude and Latitude for LAX
city_data = ('Tokyo', 2003, 32_450, 0.66, 8014)  # A record for a city
```
Here, the tuple holds the fields for a city's name, the year, population, etc. Changing the order of elements would change their meaning.

#### **Tuples as Immutable Lists**
Tuples can also be thought of as **immutable lists**. They allow you to store multiple items, but you cannot modify their contents after creation. This immutability provides two key advantages:

- **Clarity**: When you see a tuple in code, you know that its contents will never change. This immutability enforces consistency.
- **Performance**: Since tuples are immutable, Python can optimize them more effectively. They take up less memory compared to lists of the same length.

##### Mutability of Objects within Tuples
While the tuple itself is immutable, if it contains **mutable objects** (like lists), the contents of those objects can still change.

Example:
```python
a = (10, 'alpha', [1, 2])  # Tuple contains an immutable int, string, and a mutable list
b = (10, 'alpha', [1, 2])

a == b  # True, since both tuples are equal at this point
b[-1].append(99)  # Mutating the list inside the tuple
a == b  # False, because the list inside b has changed
b  # Output: (10, 'alpha', [1, 2, 99])
```
Here, although the tuple is immutable, the list inside it can still be modified.
![image](https://github.com/user-attachments/assets/8cd5b8dc-b6dc-4db8-8091-b04e055311f3)

### Unpacking Sequences and Iterables
In Python, you can **unpack** sequences and iterables directly into variables. This is useful when you know the structure and length of the sequence.

Example:
```python
lax_coordinates = (33.9425, -118.408056)
latitude, longitude = lax_coordinates  # Unpacking tuple into two variables

city_data = ('Tokyo', 2003, 32_450, 0.66, 8014)
city, year, pop, chg, area = city_data  # Unpacking all elements
``` 

You can also unpack with wildcards:
```python
a, *rest = range(5)
# a = 0, rest = [1, 2, 3, 4]
``` 

This technique simplifies working with sequences and allows for clean, readable code when handling structured data.


Example:
```
>>> a = (10, 'alpha', [1, 2])
>>> b = (10, 'alpha', [1, 2])
>>> a == b
True
>>> b[-1].append(99)
>>> a == b
False
>>> b
(10, 'alpha', [1, 2, 99])
```
