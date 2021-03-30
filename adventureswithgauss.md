# Adventures with Gauss and a partial pivot

_Written while I was a Python and Algebra tutor in 2017_

Recently a student asked if I could help her understand, step by step, the Gaussian Elimination with Partial Pivoting algorithm. 

## a partial pivot
Personally, this experience taught me that matrix operations themselves are not too bad on pen and paper. However, the matrix operations in Python are an indexing nightmare. 

## adventures with Gauss
The point of this blog article is to instill advice regarding matrices in Python to reduce stress, dyslexia and ultimately understand this algorithm. Here are the take home points we discussed: 

1. Ignore the code and solve the matrix using Gaussian Elimination with Partial Pivoting by hand first. This helps us visualize how the algorithm will operate. I like the videos from numericalmethodsguy the best. See, [Gaussian Elimination with Partial Pivoting](https://www.youtube.com/watch?v=euIXYdyjlqo).

1. Write down what i, j, and k equal for a few of the iterations and circle the affected items of the matrix on pen and paper. The key is to cozy up to the algorithm, really get to know it, and to not expect the matrix operations to jump out from the algorithm without a bit of effort on our end. This step reduces index induced dyslexia.

1. After we get to know the algorithm for a few iterations, forget about the indexes. Print out the matrix after every operation to see the algorithm in action and to troubleshoot the algorithm. Watching the matrix change via the printed output is easier than attempting to mentally keep track of the indexes. This step keeps us sane.

Here is a refactored and print() heavy sample we worked with:

## the code
```python
print("Guassian Elimination with Partial Pivot")
#Tested Python 3

import numpy as np

#Start of parameters to change.
#Change this to any n x n matrix.
A = [[ 1,  2,  0],
     [ 0,  4, -1],
     [ 2,  2,  0]]
#End of parameters to change.

def print_matrix(A):
    return ''.join([''.join(['{:8}'.format(i) for i in r]) for r in A])

n = len(A)
npivot = 0

print("Original matrix: {0}".format(print_matrix(A)))

#Numpy sanity check
detA = np.linalg.det(A)

for k in range(0, n):
    mx = abs(A[k][k])
    rowmax = k
    for i in range(k+1, n): 
        if abs(A[i][k]) > mx:
            mx = abs(A[i][k])
            rowmax = i
    if rowmax != k: 
        npivot += 1
        store = A[k]
        A[k]=A[rowmax]
        A[rowmax]=store

    print("after row switch: {0}".format(print_matrix(A)))
    
    for i in range(k+1, n): 
        factor = A[i][k]/A[k][k] 
        for j in range(k, n): 
            A[i][j] = A[i][j] - factor*A[k][j]

    print("after eliminating: {0}".format(print_matrix(A)), end="")
    
print(""upper triangluar matrix: {0}"".format(print_matrix(A)))

deter = 1
for i in range(0, n): 
    deter = deter * A[i][i]
deter = deter * (-1)**npivot

print("our determinant: ", deter)
print("numpy's determinant: ", detA)
```
