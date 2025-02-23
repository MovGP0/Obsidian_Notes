---
aliases:
  - Rotation Matrix
  - Rotation Matrices
---
A rotation matrix can be constructed from the [[Rotation using Axis-Angle Vector]].

## Rotation matrices in $SO(2)$

In the 2d-space, the rotation matrices are given by:
$$R_{Θ} = \begin{pmatrix}
\cos{Θ} & -\sin{Θ} \\
\sin{Θ} & \cos{Θ}
\end{pmatrix}$$
The inverse of the rotation matrix is the transpose of the matrix:

$$R^{-1} = R^T$$
$$R\,R^T = R^T\,R = 1$$
Also, a rotation matrix has a determinant equal to 1, meaning that it does not scale the rotated objects:
$$\det{R} = 1$$
When a matrix has this properties, the rotation matrix in 2d-space is considered to be a **Special Orthogonal 2x2 Matrix** or $SO(2)$-Matrix.

- **Orthogonal** means matrices that represent rotation and/or reflections
- **Special** means matrices that represent rotations only (without reflections)

## Scaling matrices

If the matrix is modified, such that the [[Determinant]] is no longer equal to 1 (rotation + scaling of the vector field), it is no longer considered to be an $SO(2)$ matrix and therefore not considered to be a *rotation matrix*.

If the determinant is negative, it represents a **reflection** of the object. In the special case of an matrix $M$ with determinant of $\det{M} = -1$, it is considered to be in the group $S(n)$, but not in $SO(n)$. 

> [!seealso]
> [[Scaling Matrices]]

## Skewing matrices

If the matrix is skewing an vector field (skew factor $k \ne 1$), it is not considered to be an orthogonal matrix.

> [!seealso] 
> [[Skewing Matrices]]

## Rotation matrices in $SO(3)$

An [[Rotation using Axis-Angle Vector|Axis-Angle Vector]] $K$ in 3D-space can be transformed into a rotation matrix $R$ using the [Rodrigues Formula](https://en.wikipedia.org/wiki/Rodrigues%27_rotation_formula):

$$R = I + (\sin{θ})\,K + (1-\cos{θ})\,K^2$$
where
$$I=\begin{bmatrix}
1&0&0 \\
0&1&0 \\
0&0&1
\end{bmatrix}$$
and
$$K = \begin{bmatrix}
0 & -\hat{e}_{z} & \hat{e}_{y} \\
\hat{e}_{z} & 0 & -\hat{e}_{x} \\
-\hat{e}_{y} & \hat{e}_{x} & 0
\end{bmatrix}$$
## Rotation matrices in $SU(2)$

$SU(2)$ is a generalization of $SO(2)$ using [[Complex Numbers]]:

|                           $SO(2)$                            |                           $SU(2)$                           |
| :----------------------------------------------------------: | :---------------------------------------------------------: |
|     $R = \begin{bmatrix} a & -b \\ b & a \end{bmatrix}$      | $U = \begin{bmatrix} α & -β^{*} \\ β & α^{*} \end{bmatrix}$ |
|                          $a,b\inℝ$                           |                          $α,β\inℂ$                          |
|                         $a^2+b^2=1$                          |                   $\|α\|^2 + \|β\|^2 = 1$                   |
|                    $\det{R}=1$ (Special)                     |                   $\det{U} = 1$ (Special)                   |
|                 $R^{-1} = R^T$ (Orthogonal)                  |              $U^{-1} = U^{\dagger}$ (Unitary)               |
|                    $\vec{v}' = R\vec{v}$                     |                           $s'=Us$                           |
| $\vec{v} = \begin{bmatrix}v_{1}\\v_{2}\end{bmatrix} \in ℝ^2$ |    $s=\begin{bmatrix}s_{1}\\s_{2}\end{bmatrix} \in ℂ^2$     |
The value $s$ is the [[Spinors|Spinor]].

The **magnitude** of the spinor $s$ is calculated using the [Pythagorean theorem](https://en.wikipedia.org/wiki/Pythagorean_theorem)

$$|s|^2 = |s_{1}|^2 + |s_{2}|^2 = \mathrm{Re}(s_{1})^2 + \mathrm{Im}(s_{1})^2 + \mathrm{Re}(s_{2})^2 + \mathrm{\mathrm{Im}}(s_{2})^2$$
The group $SU(2)$ **double covers** the group $SO(3)$: for every possible rotation in $SO(3)$ we have *exactly two* possible rotations in $SU(2)$; related by a $\pm$ sign:

$$v' \leftrightarrow \pm s'$$
$$R \leftrightarrow \pm U$$
Because $SO(3)$ is fully contained in $SU(2)$, we can use $SU(2)$ to represent rotations in $SO(3)$.

> [!Note]
> $SU(2)$ can be used where the orientation needs to be preserved after a (series of) rotations.
> This applies also to physical problems, like the wave function of an electron.

> [!info] 
> For computational efficiency, the [[Rotation using Quaternions|Qaternion representation]] of $SU(2)$ is preferred.
> Code using the $SU(2)$ representation is faster and requires less memory.

 > [!tip] 
 > $SO(3)$ is typically easier to think with when handling rotations in $ℝ^3$, since it doesn't involve rotations an a hypershpere.

> [!warning]
> $SO(3)$ representation has the potential issue of [Gimbal Lock](https://en.wikipedia.org/wiki/Gimbal_lock).
> Use $SU(2)$ where Gimbal Lock might be an issue.
