## Vector Addition

Addition of vectors
$$ c = a + b $$

Equals the addition of the components
$$ ( a_1+b_1, a_2+b_2, \ldots, a_n+b_n ) = ( a_1, a_2, \ldots, a_n ) + ( b_1, b_2, \ldots, b_n ) $$
$$ c = \sum_i (a_i + b_i) e_i $$

Component notation
$$ c_i = a_i + b_i $$

## Length of Vector

Using the Euclidian (L2) Norm

$$ |a|^2 = a_i\,a_i = \sum_i a_i^2 $$

## Unit Vector

$$ a_u = \frac{a}{|a|} $$

## Scalar Product / Dot Product

Scalar product
$$ c = a\,b $$
$$ c_i = a_i\,b_i $$

Magnitude and Angle of 2D vector
$$ \forall a,b\in\mathbb{R}^2: a\,b = |a|\,|b|\,\cos\gamma $$
$$ \cos\gamma = \frac{a\,b}{|a|\,|b|} $$

## Exterior Product

Also known as **wedge product**.

Higher order version of the [cross product](#cross-product)
$$ c_i = ε_{ijkl} \, a_j \, b_k \, e_l $$

### Cross product

Determinant of two 3D vectors. Returns a **pseudo-vector**.

$$ c_i = ε_{ijk} \, a_i \, b_j $$

Alternative syntax:

$$ a\times b =
\begin{bmatrix}
a_y b_z - a_z b_y \\
a_z b_x - a_x b_z \\
a_x b_y - a_y b_x
\end{bmatrix}$$

### 2D exterior product

Special case of the cross product, where all z-coordinates are 0.

$$a\times b 
= \det\begin{pmatrix} a_x & b_x \\ a_y & b_y \end{pmatrix}
= a_x b_y - a_y b_x$$

## Hadamard product

also known as **schur product** or **component-wise product**

$$c_{ij} = a_{ij} b_{ij}$$

$$a\circ b = $$

## Wedge product

Returns a **bivector**, where the basis represents the planes that are spanned by the basis of the original vector.

$$(a\wedge b)_i = ε_{ijk}\ a_j e_j\ b_k e_k$$

### Example

$$(a_x a_y a_z)\wedge (b_x b_y b_z) = \\
yz(a_y b_z - a_z b_y) +\\
zx(a_z b_x - a_x b_z) +\\
xy(a_x b_y - a_y b_x)$$

## Vector product

$$ab = a\cdot b + a\wedge b$$

Assuming the basis vectors have a length of 1 and are orthorgonal to each other, then
- 1D vector multiplication returns a scalar
- 2D vector multiplication returns a complex number
- 3D vector multiplication returns a quaternion

### Example

$$(a_x e_x + a_y e_y + a_z e_z) (b_x e_x + b_y e_y + b_z e_z) = \\
a_x b_x e_x e_x + a_x b_y e_x e_y + a_x b_z e_x e_z + \\
a_y b_x e_y e_x + a_y b_y e_y e_y + a_y b_z e_y e_z + \\
a_z b_x e_z e_x + a_z b_y e_z e_y + a_z b_z e_z e_z $$

with $xy = -xy$ and $xx = 1$ follows:

$$\overbrace{
    a_xb_x + a_yb_y + a_zb_z
}^{\text{dot product}} +
\overbrace{
    e_ye_z (a_y b_z - a_z b_y) +
    e_ze_x (a_z b_x - a_x b_z) +
    e_xe_y (a_x b_y - a_y b_x)
}^{\text{cross product}}$$

using the following bivector basis

$$e_x e_y = -(e_y e_x) = i$$
$$e_z e_x = -(e_z e_x) = j$$
$$e_y e_z = -(e_y e_z) = k$$

we get an quaternion:

$$(a_xb_x + a_yb_y + a_zb_z) +\\
(a_x b_y - a_y b_x)i +\\
(a_z b_x - a_x b_z)j +\\
(a_y b_z - a_z b_y)k$$

## Clifford Algebra

### 2D VGA Multivector
$$(1, x, y, xy)$$

Mapping:
- 2D Vector (x,y) $\Leftrightarrow$ (x,y) 2D Vector
- Pseudoscalar (1) $\Leftrightarrow$ (xy) 2d Bivector
- Complex Number (1,i) $\Leftrightarrow$ (1,xy) 2d Rotor

### 3D VGA Multivector
$$(\underbrace{1}_\text{scalar}, \underbrace{x, y, z}_\text{vector}, \underbrace{yz, zx, xy}_{bivector}, \underbrace{xyz}_\text{trivector})$$

Mapping:
- 3D Vector (x, y, z) $\Leftrightarrow$ (x, y, z) 3D Vector
- Pseudovector (x, y, z) $\Leftrightarrow$ (yz, zx, xy) 3D Bivector
- Qaternion (1, i, j, k) $\Leftrightarrow$ (1, yz, zx, xy) 3D Rotor
- Pseudoscalar (1) $\Leftrightarrow$ (xyz) 3D Trivector

## Sources

- [Freya Holmér, Why can't you multiply vectors?](https://www.youtube.com/watch?v=htYh-Tq7ZBI)