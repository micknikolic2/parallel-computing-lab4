# COMP-5002 – Lab 4 • Parallel Algorithm with MPI Collectives (Numerical Integration)

**Module** Module 6. MPI Collective Communication and Algorithms  
**Objective** Implement a parallel numerical integration to estimate Pi using MPI collectives (`bcast`, `reduce`) for an efficient distributed computation.

## Prerequisites

- Python 3 installed.
- An MPI implementation installed (Open MPI or MPICH).
- `mpi4py` installed (`pip install mpi4py`).
- Git basics: `clone`, `add`, `commit`, `push`.
- Concepts from Modules 5 and 6:
  - MPI environment (`COMM_WORLD`, rank, size).
  - Motivation for collectives.
  - `comm.bcast` and `comm.reduce`.
  - Basic data decomposition strategies.

## Background

We approximate Pi via the integral of `f(x) = 4 / (1 + x*x)` on `[0, 1]`.  
Using the trapezoidal rule with `N` intervals of width `h = (b − a)/N`:
s
