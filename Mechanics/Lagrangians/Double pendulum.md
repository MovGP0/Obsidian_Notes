**Determine the Position of Each Mass**

The position of $m_1$ is:
$$x_1 = l_1 \sin(\theta_1)$$
$$y_1 = -l_1 \cos(\theta_1)$$

The position of $m_2$ relative to $m_1$ is:
$$x_2 = x_1 + l_2 \sin(\theta_2)$$
$$y_2 = y_1 - l_2 \cos(\theta_2)$$

**Determine the Velocities of Each Mass**

$$\dot{x}_1 = l_1 \cos(\theta_1) \dot{\theta}_1$$
$$\dot{y}_1 = l_1 \sin(\theta_1) \dot{\theta}_1$$

$$\dot{x}_2 = \dot{x}_1 + l_2 \cos(\theta_2) \dot{\theta}_2$$
$$\dot{y}_2 = \dot{y}_1 + l_2 \sin(\theta_2) \dot{\theta}_2$$

**Determine the Kinetic Energy**

$$T = \frac{1}{2} m_1 (\dot{x}_1^2 + \dot{y}_1^2) + \frac{1}{2} m_2 (\dot{x}_2^2 + \dot{y}_2^2)$$

**Determine the Potential Energy** (under gravity)

$$U = m_1 g y_1 + m_2 g y_2$$

**Apply the Lagrangian equation**

$$L = T - U$$

*TODO: check this*
$$L = \frac{1}{2} m_1 l_1^2 \dot{\theta}_1^2 + \frac{1}{2} m_2 [(\dot{x}_1 + l_2 \cos(\theta_2) \dot{\theta}_2)^2 + (\dot{y}_1 + l_2 \sin(\theta_2) \dot{\theta}_2)^2] + m_1 g l_1 \cos(\theta_1) + m_2 g (l_1 \cos(\theta_1) + l_2 \cos(\theta_2))$$

**Apply the Euler-Lagrange Equation**

$$\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}_i} \right) - \frac{\partial L}{\partial q_i} = 0$$

**Compute the terms**

*TODO: check this*

| | $${\theta}_1$$ | $${\theta}_2$$ |
|--|--|--|
| $$\frac{\partial L}{\partial \dot{\theta}_i}$$ | $$ m_1 l_1^2 \dot{\theta}_1 + m_2 l_1 \dot{\theta}_1 $$ | $$m_2 l_2^2 \dot{\theta}_2 + m_2 l_1 l_2 \dot{\theta}_1 \cos(\theta_1 - \theta_2)$$ |
| $$ \frac{\partial L}{\partial \theta_i}$$ | $$ m_2 l_1 l_2 \dot{\theta}_2^2 \sin(\theta_1 - \theta_2) - (m_1 + m_2) g l_1 \sin(\theta_1) $$ | $$-m_2 l_1 l_2 \dot{\theta}_1^2 \sin(\theta_1 - \theta_2) - m_2 g l_2 \sin(\theta_2)$$ |
| $$ \frac{d}{dt} \left( \frac{\partial L}{\partial \dot{\theta}_1} \right)$$ | $$m_1 l_1^2 \ddot{\theta}_1 + m_2 l_1 \ddot{\theta}_1 $$ | $$m_2 l_2^2 \ddot{\theta}_2 - m_2 l_1 l_2 \dot{\theta}_1 \dot{\theta}_2 \sin(\theta_1 - \theta_2)$$ |

