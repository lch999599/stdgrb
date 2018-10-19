# stdgrb: Python Gurobi solvers for standard dense problems 

This is a fast cython wrapper for strandard dense optimization that calls 
gurobi through its C interface. It is more efficient for dense problems 
than the gurobi python 
interface that cannot accept directly numpy arrays for constraints.

## Standard solver

### Linear program `lp_solve`

```python
lp_solve(c,A=None,b=None,lb=None,ub=None,nbeq=0, method=-1,logtoconsole=1, crossover=-1)
```

Solves the following optimization problem:

<img src="data:image/png;base64,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">



Parameters:
* c : (d,) ndarray, float64
    Linear cost vector
* A : (n,d) ndarray, float64, optional
    Linear constraint matrix
* b : (n,) ndarray, float64, optional
    Linear constraint vector
lb : (d) ndarray, float64, optional
Lower bound constraint
ub : (d) ndarray, float64, optional
Upper bound constraint
method : int, optional
Selected solver from * -1=automatic (default), * 0=primal simplex, * 1=dual simplex, * 2=barrier, * 3=concurrent, * 4=deterministic concurrent, * 5=deterministic concurrent simplex
logtoconsole : int, optional
If 1 the print log in console,
crossover : int, optional
Select crossover strategy for interior point (see gurobi documentation)


Returns:
x: (d,) ndarray
Optimal solution x
val: float
optimal value of the objective (None if optimization error)


## Install

The only strong dependencies are

* numpy
* cython (for the compilation)

In order to build install ```stdgrb``` you need to define first environement 
variables ```GUROBI_HOME``` and ```GUROBI_VERSION``` so that the compiler can 
find the include files and link the good library.

this can be done with 

```bash
export GUROBI_HOME=/path/to/gurobi/linux64
export GUROBI_VERSION=gurobi75 # for gurobi 7.5
``` 

you can find the gurobi version in the ```GUROBI_HOME/lib``` folder by seeking 
the ```libgurobi[XX].so``` and setting ```GUROBI_VERSION=gurobi[XX]``` 
(be carefull to remove the 'lib').

When the environement variables are set, you can install the module with

```bash
python setup.py install # --user if local install
```

## Use the module


Supposing that the matrices A,b,, lb and ub are alerday defined, you can solve
 the linear program with
 
 
 ```python
 import stdgrb
 sol,val=stdgrb.lp_solve(c,A,b,lb,ub)
 ```
 
 if A,b,ul,ub and not given or None, the corresponding constraints are not 
 used.
 
 
 
