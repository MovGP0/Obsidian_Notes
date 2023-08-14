The [Atwood machine](https://en.wikipedia.org/wiki/Atwood_machine) is defined by:

Kinetic energy: $$T_i = \frac{1}{2} m_i\ \dot{x}_i^2$$
Potential Energy: $$V_i = m_i g x_i$$
Constraint by pully: $$l = x_1 + x_2 \rightarrow \dot{x}_2 = \dot{x}_1$$

## Solving the [[Lagrange Equation]]

Lagrange equation:
$$L = T - V$$

Substitution:
$$L = \frac{1}{2} m_1 \dot{x}_1^2 + \frac{1}{2} m_2 (-\dot{x}_1)^2 - m_1 g x_1 - m_2 g (l - x_1)$$

Simplifying:
$$L = \frac{1}{2} m_1 \dot{x}_1^2 + \frac{1}{2} m_2 \dot{x}_1^2 - m_1 g x_1 - m_2 g l + m_2 g x_1$$

Rearranging:
$$L = \frac{1}{2} (m_1 + m_2) \dot{x}_1^2 - (m_2 - m_1) g x_1 - m_2 g l$$

## Solving the [[Euler-Lagrange equation]] for $m_1$

Euler-Lagrange equation:
$$\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}_i} \right) - \frac{\partial L}{\partial q_i} = 0$$

Partial Derivative with respect to $\dot{x}_1$ (acceleration of $m_1$):

$$\frac{\partial L}{\partial \dot{x}_1} = (m_1 + m_2) \dot{x}_1$$

Time Derivative of the above expression:
$$\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{x}_1} \right) = (m_1 + m_2) \ddot{x}_1$$

Substitution:
$$(m_1 + m_2) \ddot{x}_1 + (m_2 - m_1) g = 0$$

Rearranging:
$$\ddot{x}_1 = \frac{(m_1 - m_2) g}{m_1 + m_2}$$

## Pulley with Inertia $I$

**Kinetic Energy**

Kinetic energy of the pulley:
$$T_{\text{pulley}} = \frac{1}{2} I \omega^2$$
Where $\omega$ is the angular velocity of the pulley.

The angular velocity of the pulley is constrained to the speed of the string $v$ and the radius of the pulley $r$:
$$\omega = \frac{v}{r} = \frac{\dot{x}_1}{r} $$

Substitution:
$$T_{\text{pulley}} = \frac{1}{2} I \left( \frac{\dot{x}_1}{r} \right)^2$$

Total kinetic energy:
$$T = \frac{1}{2} m_1 \dot{x}_1^2 + \frac{1}{2} m_2 \dot{x}_2^2 + \frac{1}{2} I \left( \frac{\dot{x}_1}{r} \right)^2$$

Using constraint $\dot{x}_2 = -\dot{x}_1$:
$$T = \frac{1}{2} m_1 \dot{x}_1^2 + \frac{1}{2} m_2 (-\dot{x}_1)^2 + \frac{1}{2} I \left( \frac{\dot{x}_1}{r} \right)^2$$

**Potential Energy**

The potential energy remains the same as before since the pulley's rotation doesn't contribute to the potential energy:
$$V = m_1 g x_1 + m_2 g x_2$$

**Lagrangian**

Substitution:
$$L = \frac{1}{2} m_1 \dot{x}_1^2 + \frac{1}{2} m_2 \dot{x}_1^2 + \frac{1}{2} I \left( \frac{\dot{x}_1}{r} \right)^2 - m_1 g x_1 - m_2 g (l - x_1)$$

Simplifying:
$$L = \left( \frac{1}{2} m_1 + \frac{1}{2} m_2 + \frac{1}{2} \frac{I}{r^2} \right) \dot{x}_1^2 - (m_2 - m_1) g x_1 - m_2 g l$$

**Euler Lagrangian**

Partial Derivative with respect to $\dot{x}_1$:
$$\frac{\partial L}{\partial \dot{x}_1} = (m_1 + m_2 + \frac{I}{r^2}) \dot{x}_1 $$

Time Derivative:
$$\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{x}_1} \right) = (m_1 + m_2 + \frac{I}{r^2}) \ddot{x}_1$$

Partial Derivative with respect to $x_1$:
$$\frac{\partial L}{\partial x_1} = -(m_2 - m_1) g$$

Substitution of derivates into Euler-Lagrange equation:
$$(m_1 + m_2 + \frac{I}{r^2}) \ddot{x}_1 + (m_2 - m_1) g = 0$$

Rearranging for $\ddot{x}_1$:
$$\ddot{x}_1 = \frac{(m_1 - m_2) g}{m_1 + m_2 + \frac{I}{r^2}}$$


