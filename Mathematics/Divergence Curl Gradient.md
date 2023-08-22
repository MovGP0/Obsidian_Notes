# Divergence

| Property/Operator | $\text{div}$ |
|-------------------|-------------------------|
| **Symbol**               | $\nabla \cdot \mathbf{F}$ |
| **Definition**           | For a vector field $\mathbf{F} = (F_x, F_y, F_z)$ in $\mathbb{R}^3$, its divergence is: $$ \text{div}(\mathbf{F}) = \frac{\partial F_x}{\partial x} + \frac{\partial F_y}{\partial y} + \frac{\partial F_z}{\partial z} $$ |
| **Output**               | Scalar field |
| **Geometric Meaning**   | Measures the rate at which "density" exits a given region (how much a field is "spreading out"). Positive divergence indicates a source, and negative indicates a sink. |
| **Physical Examples**   | 1. Divergence of a fluid velocity field gives the rate of volume expansion or contraction. 2. Electric charge density in relation to electric field in Gauss's law. |
| **Invariants**          | Divergence is coordinate invariant, meaning it remains unchanged under coordinate transformations. |
| **Fundamental Theorems**| Divergence Theorem (or Gauss's Theorem): For a vector field $\mathbf{F}$ and a volume $V$ bounded by a closed surface $S$, $$ \int_V \nabla \cdot \mathbf{F} \, dV = \oint_S \mathbf{F} \cdot d\mathbf{S} $$ |

# Curl

| Property/Operator        | $\text{rot}$ or $\text{curl}$ |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Symbol**               | $\nabla \times \mathbf{F}$                                                                                                                                                                                                                                                                |
| **Definition**           | For a vector field $\mathbf{F}$, its curl is: $$ \text{curl}(\mathbf{F}) = \nabla \times \mathbf{F} = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ \frac{\partial}{\partial x} & \frac{\partial}{\partial y} & \frac{\partial}{\partial z} \\ F_x & F_y & F_z \end{vmatrix} $$ |
| **Output**               | Vector field                                                                                                                                                                                                                                                                              |
| **Geometric Meaning**    | Measures the tendency of the field to rotate around a point. Non-zero curl indicates a vortical behavior.                                                                                                                                                                                 |
| **Physical Examples**    | 1. Circulation in a fluid flow. 2. Magnetic fields around electric currents according to Ampère's circuital law.                                                                                                                                                                          |
| **Invariants**           | Magnitude of the curl is invariant under coordinate transformations.                                                                                                                                                                                                                      |
| **Fundamental Theorems** | Stokes' Theorem: For a vector field $\mathbf{F}$ and a surface $S$ bounded by a closed curve $C$, $$ \oint_C \mathbf{F} \cdot d\mathbf{r} = \int_S (\nabla \times \mathbf{F}) \cdot d\mathbf{S} $$                                                                                        |

# Gradient

| Property/Operator | $\text{grad}$ (Gradient) |
|-------------------|------------------------------|
| **Symbol**        | $\nabla f$               |
| **Definition**    | For a scalar field $f$ in $\mathbb{R}^3$, its gradient is defined as: $$ \text{grad}(f) = \nabla f = \left( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z} \right) $$ |
| **Output**        | Vector field                 |
| **Geometric Meaning** | Points in the direction of the maximum rate of increase of the scalar field and its magnitude is the maximum rate of change at that point. |
| **Physical Examples** | 1. Gradient of a temperature field gives the direction and rate of fastest temperature increase. 2. Electric field as the gradient of voltage in electrostatics. |
| **Invariants**    | The gradient is not invariant; its expression changes under coordinate transformations. However, its magnitude and direction (as a vector) remain invariant. |
| **Fundamental Theorems** | Gradient Theorem: For a scalar field $f$ and a curve $C$ from point A to point B, $$ \int_A^B \nabla f \cdot d\mathbf{r} = f(B) - f(A) $$ |
