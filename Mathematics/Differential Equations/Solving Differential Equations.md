## Approaches

- Substitution
- Energy Conservation
- Series Expansion
- Laplace Transform
- Hamiltonian Flow

**Example:** Simple Harmonic Oscillator

$F = m \frac{\mathrm{d}^2\,x}{\mathrm{d}\,t^2} = -k\,x$

## Substitution

The idea is to guess the function and substitute.

**Ansatz:** Assume that the solution can be described by this function:

$x(t) = A \cos \left( Ω\,t \right)$

| Term | Description           | Phyical Unit |
| ---- | --------------------- | ------------ |
| x    | Position              | [m]          |
| A    | Some constant         | [m]          |
| Ω    | Oscillation Frequency | [rad/s]      |
| t    | Time                  | [s]          |

**Substitute**

$\frac{\mathrm{d}^2\,x}{\mathrm{d}\,t^2} = (-k\,m^{-1})\,x$

**Calculate 2nd derivative**

$x(t) = A\cos(Ωt) + B\sin(Ωt)$ => Apply chain rule
$\frac{\mathrm{d}\,x}{\mathrm{d}\,t^2} = −AΩ\sin(Ωt)+BΩ\cos(Ωt)$ => Apply chain rule
$\frac{\mathrm{d}^2\,x}{\mathrm{d}\,t^2} = -Ω^2 A\cos(Ωt)-Ω^2B\sin(Ωt)$

**Substitute**

$\frac{\mathrm{d}^2\,x^2}{\mathrm{d}\,t^2} = \left(-k\,m^{-1}\right)\,x = -Ω^2 \left(A\cos(Ωt) + B\sin(Ωt) \right)$

- A and B represent the initial condition of the system.

## Energy Conservation

$$E = \frac{1}{2} m \left( \frac{\mathrm{d}\,x}{\mathrm{d}\,t} \right)^2 + \frac{1}{2} k\,x^2$$

## Series Expansion

$$x(t) = \sum_{n=0}^\infty a_n\,t^n$$

## Laplace Transform

$$\hat{x}(s) = \int_0^\infty \mathrm{d}\,t\,e^{-s\,t}\, x(t)$$

## Hamiltonian Flow

$$\frac{\mathrm{d}}{\mathrm{d}\,t} \begin{pmatrix} x \\ p \end{pmatrix} = \begin{pmatrix} p\, m^{-1} \\ -k\,x \end{pmatrix}$$

## Sources

- YouTube: [Physics with Elliot](https://www.youtube.com/@PhysicswithElliot): [Physics Students Need to Know These 5 Methods for Differential Equations](https://www.youtube.com/watch?v=0kY3Wpvutfs)
