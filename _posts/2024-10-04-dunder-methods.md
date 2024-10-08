---
title: 'Dunder Methods'
date: 2024-10-04
permalink: /posts/2024/10/DunderMethods/
tags:
  - Python
  - Books
---

#### What are special methods?
They are 'predefined' methods that allow you to customize the behavior of classes. Can also be referred to as “dunder” (double-underscore) methods.

You don't write `my_object.__len__()`, you write `len(my_object)`. 

#### Difference with normal methods

- **Implicit invocation:** Special methods are called automatically by Python in specific situations, while regular methods need to be called explicitly.
- **Syntax integration:** Special methods allow your objects to work with Python's built-in syntax and functions, whereas regular methods don't.

```python
class MyClass:
    def __init__(self, value):
        self.value = value
    
    def __str__(self):
        return f"MyClass instance with value: {self.value}"
    
    def __add__(self, other):
        return MyClass(self.value + other.value)
    
    def double_value(self):
        return self.value * 2
        
# Usage obj1 = MyClass(5) 
obj2 = MyClass(10) 

print(obj1) # Calls __str__ implicitly 
result = obj1 + obj2 # Calls __add__ implicitly 
doubled = obj1.double_value() # Regular method, called explicitl
```

### Uses for Special Methods

#### 1. Emulating Numeric Types
Several special methods allow user objects to respond to operators such as '+' or `*` 
#### 2. String representation of objects
`__repr__(self):`Returns a detailed string representation of the object, used for debugging. Without a custom `__repr__`, Python’s console would display a Vector instance `<Vector object at 0x10e100070>`, but we can change this:

Example
```python
class Vector:
	def __repr__(self):
		return f'Vector({self.x!r}, {self.y!r})'

v1 = Vector(2, 4)

print(v1) # Normally prints <__main__.Vector object at 0x7dc33ea23520>

print(v1) #But now it returns Vector(2, 4)
```
#### 3. Boolean value of a Custom Type
if `__bool__` is not implemented, Python tries to invoke `x.__len__()`, and
if that returns zero, bool returns **False**. Otherwise bool returns **True**.

#### Common special methods: 
1. `__init__(self, ...)`: Constructor method, called when an object is created.

2. `__str__(self)`: Returns a string representation of the object, used by `str()` and `print()`.

3. `__repr__(self)`: Returns a detailed string representation of the object, used for debugging.

4. `__len__(self)`: Defines behavior for the `len()` function.

5. `__getitem__(self, key)`: Allows indexing or key-based access to the object.

6. `__setitem__(self, key, value)`: Allows setting values using indexing or key-based access.

7. `__iter__(self)`: Makes an object iterable.

8. `__eq__(self, other)`: Defines behavior for the equality operator (= =).

9. `__lt__(self, other)`: Defines behavior for the less than operator (<).

10. `__add__(self, other)`: Defines behavior for the addition operator (+).
