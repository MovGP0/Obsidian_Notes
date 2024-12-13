The **Spacetime Interval** $ds^2$ is a measure of the separation between two events in spacetime.

$$ds^2 = -c^2\  dt^2 + {a(t)}^2\ \left( dx^2 + dy^2 + dz^2 \right)$$

Where $a(t)$ is the scale factor for the expansion (or contraction) of the universe.

### Interpretation of the Spacetime Interval

In General Relativity, the quantity $ds^2$ encodes the nature of the separation between two events in spacetime.

It's sign and magnitude indicate whether signals or matter can travel between these events and how they fit into each other’s causal structure:

- **Timelike Interval ($ds^2<0$)**:  
    The events are separated in such a way that one could be the cause of the other.
    A massive particle, traveling at less than the speed of light, can move from one event to the other.
    In a suitable reference frame, the two events can occur at the same spatial location, differing only in time.
 
- **Spacelike Interval ($ds^2>0$)**:  
    The events are so far apart in space and so close together in time that no signal, not even light, can travel from one to the other. They are causally disconnected.
    It is possible, by changing reference frames, to make these events occur simultaneously—though at different points in space.
 
- **Null (Lightlike) Interval ($ds^2=0$)**:  
    The events are connected by a path that a light ray could follow.
    They lie exactly on each other’s light cones.
    Light (or anything traveling at the speed of light) can move from one event to the other, but no slower-than-light particle can do so directly.

## Spacetime Interval with moving observer

The line describing the distance between two events for an observer 'at rest' is given by

$$ds^2 = -c^2\,dt^2 + {\color{brown} a(t)}^2\,\left( dx^2 + dy^2 + dz^2 \right)$$
$$ds^2 = -c^2\,dt^2 + {\color{brown} (1+H\,dt)}^2\,\left( dx^2 + dy^2 + dz^2 \right)$$
For a observer that moves uniformly with the speed $v$, we need to apply the Galilean Transformation:

$$ x' = x-vt $$
$$t' = t$$

$$\begin{align*}
ds'^2 &= -c^2\,dt'^2 + (1 + H\,dt')^2\,dx'^2 \\
&\approx -c^2\,dt^2 + (1 + 2\,H\,dt)\,dx'^2 \\
&= -c^2\,dt^2 + (1 + 2\,H\,dt)\,(dx^2 + v^2\,dt^2 - 2\,v\,dx\,dt)
\end{align*}$$

### Generalization to 3 spatial dimensions

With
$$
\begin{align*}
\mathbf{x}^2 &= dx^2 + dy^2 + dz^2 \\
\mathbf{v} &= (v_x, v_y, v_z) \\
\mathbf{v} \cdot d\mathbf{x} &= v_x\,dx + v_y\,dy + v_z\,dz
\end{align*}$$
we get
$$
\begin{align*}
ds'^2 &= -c^2\,dt^2 + (1 + 2\,H\,dt)\left(d\mathbf{x}^2 + v^2 dt^2 - 2(\mathbf{v}\cdot d\mathbf{x})\,dt\right) \\
&= -c^2 dt^2 + (1 + 2\,H\,dt) \left( dx^2 + dy^2 + dz^2 + (\mathbf{v}\cdot\mathbf{v})\,dt^2 - 2(\mathbf{v}\cdot d\mathbf{x})\,dt \right)
\end{align*}$$
## Time Dilation

The term $H\,dt$ can be ignored for human timescales, because of the relatively large age of the universe. Therefore we get:

$$ds'^2 = -c^2\,\left( 1-\frac{v^2}{c^2} \right)\,dt^2 - 2\,v\,dt + dx^2$$
The [[Metric Tensor of the Universe]] under Galilean Transformation represents **Time Dilation**:

$$g = \begin{pmatrix}
-c^2 & 0 \\
0 & 1 \\
\end{pmatrix}
\ \Rightarrow\ 
g' = \begin{pmatrix}
-c^2\,\left( 1-\frac{v^2}{c^2} \right) & -\frac{v}{c} \\
-\frac{v}{c} & 1 \\
\end{pmatrix}
$$

