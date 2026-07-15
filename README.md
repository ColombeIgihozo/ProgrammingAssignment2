# Caching the Inverse of a Matrix

This repository contains an R programming assignment for the
[R Programming](https://www.coursera.org/learn/r-programming) course
on Coursera, part of the Johns Hopkins Data Science Specialization.

## Overview

Matrix inversion is usually a costly computation, and there may be
benefit to caching the inverse of a matrix rather than computing it
repeatedly. This assignment implements a pair of functions that cache
the inverse of a matrix using R's lexical scoping rules, so that if the
inverse has already been calculated for a given matrix (and the matrix
hasn't changed), it can be looked up in the cache instead of being
recomputed.

## Files

- **`cachematrix.R`** — contains the two functions described below.

## Functions

### `makeCacheMatrix(x = matrix())`

Creates a special "matrix" object (really a list of functions) that can
cache its inverse. It returns a list containing four functions:

| Function        | Description                              |
|-----------------|-------------------------------------------|
| `set(y)`        | sets the value of the matrix, clears the cached inverse |
| `get()`         | returns the current matrix                |
| `setinverse(inv)` | stores the computed inverse in the cache |
| `getinverse()`  | returns the cached inverse (or `NULL` if not yet computed) |

### `cacheSolve(x, ...)`

Computes the inverse of the special "matrix" object returned by
`makeCacheMatrix`. It first checks whether the inverse has already been
cached:

- If a cached inverse exists, it prints `"getting cached data"` and
  returns the cached value, skipping the computation.
- Otherwise, it computes the inverse using `solve()`, stores it in the
  cache via `setinverse()`, and returns the result.

`...` arguments are passed through to `solve()`.

**Note:** this assignment assumes the matrix supplied is always
invertible (square and non-singular).

## Usage

```r
source("cachematrix.R")

## Create a special matrix
m <- matrix(c(2, 0, 0, 0, 2, 0, 0, 0, 2), nrow = 3)
cm <- makeCacheMatrix(m)

## First call: computes and caches the inverse
cacheSolve(cm)

## Second call: retrieves the cached inverse (prints "getting cached data")
cacheSolve(cm)

## Assigning a new matrix clears the cache
cm$set(matrix(c(4, 0, 0, 4), nrow = 2))
cacheSolve(cm)  # recomputes, since the underlying matrix changed
```

## Example Output

```
> cacheSolve(cm)
     [,1] [,2] [,3]
[1,]  0.5  0.0  0.0
[2,]  0.0  0.5  0.0
[3,]  0.0  0.0  0.5

> cacheSolve(cm)
getting cached data
     [,1] [,2] [,3]
[1,]  0.5  0.0  0.0
[2,]  0.0  0.5  0.0
[3,]  0.0  0.0  0.5
```

## Acknowledgements

Assignment instructions and the `makeVector`/`cachemean` example
adapted from the
[R Programming course](https://github.com/rdpeng/ProgrammingAssignment2)
by Roger D. Peng, Johns Hopkins Bloomberg School of Public Health.
