---
title: 'Introduction to Unittesting'
date: 2024-12-09
permalink: /posts/2024/12/intro-unittest/
tags:
  - Python
---

### Why use Unit Testing?
*Unittesting* for any serious project is mandatory, not optional. It's a good negative indicator -i.e. it points out poor-quality code with relatively high accuracy-. If the code is hard to unit test, it's a strong sing that the code needs improvement. 

The goal is to enable sustainable growth of the software project. It's easy to grow a project. It's much harder to sustain this growth over time. 

```tikz
\begin{document}
  \begin{tikzpicture}
    % Draw the grid
    \draw[very thin, color=gray] (-0.2,-0.2) grid (4.2,4.2);
    
    % Draw the axes
    \draw[->] (-0.2,0) -- (4.5,0) node[right] {Progress};
    \draw[->] (0,-0.2) -- (0,4.5) node[above] {Work hours spent};
    
    % Plot the "Without tests" curve with a capped domain
    \draw[color=red, thick, smooth] 
      plot[domain=0:2.8] (\x, {0.2*\x^3}) 
      node[right] {Without tests};
      
    % Plot the "With tests" curve
    \draw[color=blue, thick, dashed, smooth] 
      plot[domain=0:4] (\x, {0.8*\x}) 
      node[right] {With tests};
  \end{tikzpicture}
\end{document}
```

> A *regression* is when a feature stops working as intended after a certain event (usually, a code modification). The terms *regression* and *software bug* are synonyms. 

> It's easy to create tests whose net value is close to zero. We need to consider both the test's value and its upkeep cost.  


### Coverage Metrics to measure test suite quality

**CODE COVERAGE**

A *coverage metric* shows how much source code a test suite executes, from none to 100%. The common belief is that the **higher** the coverage number, the **better** 

$$
\text{Code coverage (test coverage) = } \frac{\text{Lineas of code exectuded}}{\text{Total number of lines}}
$$

**BRANCH  COVERAGE** 

Another coverage metric. Branch coverage provides more precise results than code coverage because it helps cope with the code coverage's shortcomings. Instead of using the raw number of code lines, this metric focuses on control structures, such as `if` and `switch` statements. It shows how many of such control structures are traversed by at least one test in the suite. 

$$
\text{Branch coverage} = \frac{\text{Branches traversed}}{\text{Total number of branches}}
$$

### Useful Definitions
- *Unit test*: A test that checks a small bit of code, like a function or a class, in isolation of the rest of the system. 
- *Integration test*: A test that checks a larger bit of code, maybe several classes, or a subsystem. Still is smaller than a system test
- *System test*: A test that checks all of the system under test in an environment as close to the end-user environment as possible. 
- *Functional test*: A test that checks a single bit of functionality of a system. 
- *Subcutaneous test*:  A test that doesn't run against the final end-user interface, but against an interface just below the surface. 

### Marking Test Functions
Markers make sense after seeing them in action. Let's say we want to run a subset of our test to get a sense for whether or not there is some major break in the system. Smoke test are by convention not all-inclusive, thorough test suites, but a select subset that can be run quickly and give a developer an idea of the health of all parts of the system. 

To add a smoke test to we can add the decorator `@mark.pytest.smoke` to some of the test. 

```python 
@pytest.mark.smoke
def test_list_raises(): 
 """list() should raise an exception with wrong type param."""
 with pytest.raises(TypeError):
  task.list_task(owner=123)
  
@pytest.mark.get 
@pytest.mark.smoke 
def test_get_raises(): 
 """get() should raise an exception with wrong type param.""" 
  with pytest.raises(TypeError):
   tasks.get(task_id='123')
```

To run them 

```bash
pytest -v -m 'smoke' test_api_exceptions.py
```

Additionally, the expression after -m can use and, or, and not to combine multiple markers:

`pytest -v -m 'smoke and get' test_api_exceptions.py` will run only the test that had both *smoke* and *get* markers. 

### Skipping Tests

While the markers were names of your own choosing, pytest includes a few helpful builtin markers: `skip, skipif, xfail` markers enable you to skip test you don't want to run.  for example, 

```python 
@pytest.mark.skip(reason='misunderstood the API') 
def test_unique_id_1(): 
"""Calling unique_id() twice should return different numbers.""" 
 id_1 = tasks.unique_id() 
 id_2 = tasks.unique_id() 
 assert id_1 != id_2
```

Now when we run `pytest -v test_unique_id_2.py` this test gets `SKIPPED` 

With the `skip` and `skipif` markers, a test isn’t even attempted if skipped. With the `xfail` marker, we are telling pytest to run a test function, but that we expect it to fai
```bash
$ cd /path/to/code/ch2/tasks_proj/tests/func 
$ pytest test_unique_id_4.py 
======================== test session starts ================= 
collected 4 items 
test_unique_id_4.py xxX. 
========== 1 passed, 2 xfailed, 1 xpassed in 0.07 seconds =========== 

The x is for XFAIL, which means “expected to fail.” The capital X is for XPASS or “expected to fail but passed.”
``` 

### Running Tests

**A Single Test File/Module** 
To run a file full of tests, list the file with the relative path as a parameter to pytest:
`$ pytest tests/func/test_add.py`

**A Single Test Function or A Single Test Class** 
To run a single test function, add :: and the test function name:
`$ pytest -v tests/func/test_add.py::test_add_returns_valid_id`
 This also works for classes
```python

class TestUpdate():
 def test_bad_id(self):
   # code
 def test_bad_task(self):
   # code
```

`$ pytest -v tests/func/test_add.py::TestUpdate`

**A Single Test Method of a Test Class**
For a single method in the class and not all of a test class we can add another `::` 
`$ pytest -v test/func/test_api_excepctions.py::TestUpdate::test_bad_id`


