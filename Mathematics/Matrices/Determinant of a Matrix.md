
The **Determinant** describes the scaling of a vector field when the matrix is applied.

## Determinant of an 2x2 Matrix

$$\det(A) = \det\begin{bmatrix}
a&b \\
c&d
\end{bmatrix} = \left(a\cdot d - b\cdot c\right)$$


## Determinant of an 3x3 Matrix

$$\det(A) = \det\begin{bmatrix}
a&b&c \\
d&e&f \\
g&h&i
\end{bmatrix} = aei + bdi + ceg - afh - bdi - ceg$$

## Determinants of larger matrices

For larger matrices, you use Levi-Civita's expansion by minors.

The determinant is calculated using:

$$\det(A) = a_1\cdot\det(M_{11})-a_2\cdot\det(M_{12})+\cdots+(-1)^n\cdot a_n\cdot\det(M_{nn})$$

where $M_{ij}$ is the minor of $A_{ij}$ (obtained by deleting row $i$ and column $j$).

## Volume of a Vector Space

The volume that is spanned by $n$ **linearly independent** vectors $a,b,c,\dots\inℝ^n$ is given by

$$V = \left|\begin{array}{ccc}
\mathbf{i} & \mathbf{j} & \mathbf{k} & \dots & v_{n}\\
a_1 & a_2 & a_3 & \dots & a_{n} \\
b_1 & b_2 & b_3 & \dots & b_{n} \\
c_1 & c_2 & c_3 & \dots & c_{n} \\
\dots & \dots & \dots & \dots & \dots
\end{array}\right|$$
- A **positive determinant** indicates that vectors **form a volume** (for volumes of paralleloged) or an orientation of space (for orientations).
- The **absolute value** of the determinant determines the **magnitude** of the property. The sign can be used to compare the **orientation of vector spaces**.

### Orientation of a Vector Space

The **orientation** of an n-dimensional vector space is also determined by the determinant of an $n \times (n+1)$ matrix, formed by $n$ **linearly independent** vectors and one reference vector ($e = 1$):

$$\text{Orientation} = \left|\begin{array}{ccccc}
\mathbf{i} & \mathbf{j} & \cdots & \mathbf{v}_n \\
\mathbf{e_{0}} & \mathbf{e_1} & \cdots & \mathbf{e_n}
\end{array}\right|$$
