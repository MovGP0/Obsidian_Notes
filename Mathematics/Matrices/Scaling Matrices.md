**Scaling matrices** are used to resize objects by multiplying their coordinates with scale factors along different axes.

The scale transformation is **uniform** when all axes are scaled by the same factor and **non-uniform** when different axes are scaled by different factors.

## Definition

A matrix $M$ is *scaling a vector field*, if it's [[Determinant of a Matrix|Determinant]] is not equal to 1.

When the Determinant is negative, it is also **reflecting** (mirroring) the vector field.

## Scaling Matrix in $ℝ^2$

A **2D scaling transformation** scales an object along the x-axis and y-axis. The transformation matrix is:

$$S_{2D} = \begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

- $s_x$ → Scale factor along the x-axis.
- $s_y$ → Scale factor along the y-axis.
- The last row ensures that the transformation works in **homogeneous coordinates** (used in computer graphics).

**Applying the 2D Scaling Matrix**

For a point $P(x, y)$, the new coordinates after scaling are:

$$P' = \begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} = \begin{bmatrix} s_x \cdot x \\ s_y \cdot y \\ 1 \end{bmatrix}$$

## Scaling Matrix in $ℝ^3$

A **3D scaling transformation** extends the 2D scaling to include the z-axis:

$$S_{3D} = \begin{bmatrix} s_x & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 \\ 0 & 0 & s_z & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

- $s_x$ → Scale factor along the x-axis.
- $s_y$ → Scale factor along the y-axis.
- $s_z$ → Scale factor along the z-axis.

**Applying the 3D Scaling Matrix**

For a point $P(x, y, z)$, the new coordinates after scaling are:

$$P' = \begin{bmatrix} s_x & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 \\ 0 & 0 & s_z & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} = \begin{bmatrix} s_x \cdot x \\ s_y \cdot y \\ s_z \cdot z \\ 1 \end{bmatrix}$$

## Scaling Matrix in $ℝ^4$

Scaling can also be extended to **4D space**, which is useful in physics (relativity), game engines, and computer graphics for transformations in higher dimensions.

The **4D scaling transformation** matrix is:

$$S_{4D} = \begin{bmatrix} s_x & 0 & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 & 0 \\ 0 & 0 & s_z & 0 & 0 \\ 0 & 0 & 0 & s_w & 0 \\ 0 & 0 & 0 & 0 & 1 \end{bmatrix}$$

- $s_x$ → Scale factor along the x-axis.
- $s_y$ → Scale factor along the y-axis.
- $s_z$ → Scale factor along the z-axis.
- $s_w$ → Scale factor along the w-axis.

**Applying the 4D Scaling Matrix**

For a point $P(x, y, z, w)$, the new coordinates after scaling are:

$$P' = \begin{bmatrix} s_x & 0 & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 & 0 \\ 0 & 0 & s_z & 0 & 0 \\ 0 & 0 & 0 & s_w & 0 \\ 0 & 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \\ w \\ 1 \end{bmatrix} = \begin{bmatrix} s_x \cdot x \\ s_y \cdot y \\ s_z \cdot z \\ s_w \cdot w \\ 1 \end{bmatrix}$$
