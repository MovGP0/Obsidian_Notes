The **Schwarzschild metric** describes the spacetime geometry outside of a spherically symmetric, non-rotating mass.

In spacetime coordinates $(t, r, θ, ϕ)$ using natural units ($c=1$), the Schwarzschild metric can be written as follows:

$$ds^2 = -\left(1-\frac{2GM}{r}\right) dt^2 + \left(1-\frac{2GM}{r}\right)^{-1} dr^2 + r^2\, d\theta^2 + r^2\sin^2\theta\, d\phi^2$$

This corresponds to the following 4×4 matrix representation of the metric tensor $g_{\mu\nu}$:

$$g_{\mu\nu} = \begin{pmatrix}
-\left(1-\frac{2GM}{r}\right) & 0 & 0 & 0 \\
0 & \left(1-\frac{2GM}{r}\right)^{-1} & 0 & 0 \\
0 & 0 & r^2 & 0 \\
0 & 0 & 0 & r^2\sin^2\theta
\end{pmatrix}$$

Hereby is,

- **G** the gravitational constant ($6.67430(15)\cdot 10^{−11}\,\mathrm{m^3\,kg^{-1}\,s^{-2}}$)
- **M** the mass of the object
- **c** the speed of light ($299\ 792\ 458\,\mathrm{m\,s^{-1}}$)
- **t** the time coordinate.<br/>The component $g_{tt} = -\left(1-\frac{2GM}{r}\right)$ represents the time dilation effects in the gravitational field.
- **r** the radial distance from the object.<br/>The component $g_{rr} = \left(1-\frac{2GM}{r}\right)^{-1}$ accounts for the radial stretching of space.
- **θ** and **ϕ** the angles to the object.<br/>The angular parts $g_{\theta\theta} = r^2$ and $g_{\phi\phi} = r^2\sin^2\theta$ capture the geometry of the 2-sphere at radius $r$.

## Schwarzschild Radius

The **Schwarzschild Radius** is when the distance $r = \frac{2GM}{c^2}$.

## Sources

- [The Geometry of a Black Hole](https://www.youtube.com/watch?v=q6RufF4a6LM)
