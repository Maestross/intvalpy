# Interval library in Python

The Python module implements an algebraically closed system for working with intervals, solving interval systems of both
linear and nonlinear equations, and visualizing multiple solutions.

For details, see the complete documentation on [API](https://intvalpy.readthedocs.io/ru/latest/index.html).

## Installation

Make sure you have all the system-wide dependencies, then install the module itself:
```
pip install intvalpy
```

## Examples

### Visualizing solutions

We can calculate the list of vertices of the convex set described by a point the system of inequalities ``A * x >= b`` or
if an interval system of equations is considered ``A * x = b`` as well as visualize this set:

```python
import intvalpy as ip

import numpy as np
import matplotlib.pyplot as plt

fig, ax = plt.subplots(ncols=2, figsize=(15,8))

A, b = ip.Shary(2)
vertices1 = ip.IntLinIncR2(A, b, show=False)
vertices2 = ip.IntLinIncR2(A, b, consistency='tol', show=False)

A = -np.array([[-3, -1],
              [-2, -2],
              [-1, -3],
              [1, -3],
              [2, -2],
              [3, -1],
              [3, 1],
              [2, 2],
              [1, 3],
              [-1, 3],
              [-2, 2],
              [-3, 1]])
b = -np.array([18,16,18,18,16,18,18,16,18,18,16,18])
vertices3 = ip.lineqs(A, b, show=False)

for k in range(len(vertices1)):
    if len(vertices1[k])>0:
        x, y = vertices1[k][:,0], vertices1[k][:,1]
        ax[0].fill(x, y, linestyle = '-', linewidth = 1, color='gray', alpha=0.5)
        ax[0].scatter(x, y, s=0, color='black', alpha=1)

for k in range(len(vertices2)):
    if len(vertices2[k])>0:
        x, y = vertices2[k][:,0], vertices2[k][:,1]
        ax[0].fill(x, y, linestyle = '-', linewidth = 1, color='blue', alpha=0.3)
        ax[0].scatter(x, y, s=10, color='black', alpha=1)

ax[0].text(-4.5, -5.5, 'United and Tolerance sets of the system Shary',
           rotation = 0,
           fontsize = 15)      

x, y = vertices3[:,0], vertices3[:,1]
ax[1].fill(x, y, linestyle = '-', linewidth = 1, color='peru', alpha=0.3)
ax[1].scatter(x, y, s=10, color='black', alpha=1)
ax[1].text(-1.5, -7.77, 'Duodecagon',
           rotation = 0,
           fontsize = 15);
```
![SolSet](https://raw.githubusercontent.com/AndrosovAS/intvalpy/master/examples/SolSet.png)

### External decision evaluation:

To obtain an external estimate of an interval linear system of algebraic equations, we can use the method of crushing solutions PSS:

```python
import intvalpy as ip

A = ip.Interval([[3.33, 0, 0], [0, 3.33, 0], [0, 0, 3.33]],
                [[3.33, 2, 2], [2, 3.33, 2], [2, 2, 3.33]])
b = ip.Interval([-1, -1, -1], [2, 2, 2])

ip.PSS(A, b)
```

### Interval system of nonlinear equations:

This library also implements a multivariate interval Krawczyk and Hansen-Sengupta methods for solving nonlinear systems:

```python
import intvalpy as ip

epsilon = 0.1
def f(x):
    return ip.asinterval([x[0]**2 + x[1]**2-1-ip.Interval(-epsilon, epsilon),
                          x[0] - x[1]**2])

def J(x):    
    result = [[2*x[0], 2*x[1]],
              [1, -2*x[1]]]
    return ip.asinterval(result)

ip.HansenSengupta(f, J, ip.Interval([0.5,0.5],[1,1]))
```

Links
-----

* [Homepage](<https://github.com/AndrosovAS/intvalpy>)
* [Online documentation](<https://intvalpy.readthedocs.io/ru/latest/#>)
* [PyPI package](<https://pypi.org/project/intvalpy/>)
* A detailed [monograph](<http://www.nsc.ru/interval/Library/InteBooks/SharyBook.pdf>) on interval theory
