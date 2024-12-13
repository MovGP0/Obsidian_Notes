The **Friedmann-Lemaître-Robertson-Walker (FLRW) metric** describes a homogeneous and isotropic universe and is the basis for the modern theory of cosmology.

$$ ds^2 = -c^2 dt^2 +a^2(t) \left(\frac{1}{1-kr^2} dr^2 + r^2 a\theta^2 + r^2 \sin^2 \theta d\phi^2 \right)$$

The FLRW metric describes the geometry of the universe, incorporating the effects of cosmic expansion via the scale factor $a(t)$, spatial curvature $k$, and the passage of time $t$. It provides the framework for understanding the evolution of the universe and underlies all modern cosmological theories, including the Big Bang model and dark energy-dominated expansion.

----

- $-ds^2$ is the [[Spacetime Interval]]

- $-c^2 dt^2$: Time Contribution
	- This term represents the **contribution of time** to the spacetime interval.
	- $c$ is the speed of light, and $t$ is the **cosmic time**, a universal time coordinate used in cosmology.
	- The negative sign reflects the metric signature used in general relativity (often \((-+++)\)).

- $a^2(t)$: Scale Factor
	- $a(t)$ is the **scale factor**, which measures the expansion or contraction of the universe as a function of time.
	- $a(t)$ changes over cosmic time $t$, with $a(t) = 1$ at a chosen reference time (e.g., the present).
	- This factor scales the spatial part of the metric, reflecting how distances between objects change as the universe evolves.

- $\frac{1}{1-kr^2} dr^2$: Radial Contribution
	- This term represents the **radial spatial distance** in spherical coordinates, adjusted for curvature.
	- $k$ is the **curvature parameter** of the universe:
	  - $k = 0$: Flat universe (Euclidean geometry).
	  - $k > 0$: Closed universe (spherical geometry).
	  - $k < 0$: Open universe (hyperbolic geometry).
	- $r$ is the comoving radial coordinate, which remains fixed for objects moving with the cosmic flow (i.e., expanding or contracting with the universe).

- $r^2 d\theta^2 + r^2 \sin^2 \theta d\phi^2$: Angular Contribution
	- These terms represent the **angular part of the metric** in spherical coordinates:
	  - $d\theta^2$: Change in the polar angle ($\theta$).
	  - $\sin^2 \theta d\phi^2$: Change in the azimuthal angle ($\phi$).
	- Together, they describe the geometry of a sphere with radius $r$.

### Physical Interpretation as Universal Clock

The metric describes the structure of spacetime in a universe governed by general relativity under the assumptions of homogeneity (same everywhere) and isotropy (same in all directions). It has the following features:

**Cosmic Time ($t$):**  
   The coordinate $t$ is universal in the sense that it is the same for all observers who are comoving with the expansion of the universe (e.g., galaxies receding from one another).

**Expanding/Contracting Universe ($a(t)$):**  
   The scale factor $a(t)$ evolves with cosmic time, influencing spatial distances. The evolution of $a(t)$ is determined by the equations of motion derived from Einstein's field equations (e.g., the Friedmann equations).

**Spacetime Structure:**  
   The metric encapsulates how distances and times are measured in an expanding universe, providing the "cosmic clock" framework for understanding the dynamics of spacetime and its contents.

### Special Cases

**Flat Universe ($k = 0$):**

$$ds^2 = -c^2 dt^2 + a^2(t) \left(dr^2 + r^2 d\theta^2 + r^2 \sin^2 \theta d\phi^2\right)$$

This represents a spatially flat universe (consistent with current observations).

**Static Universe ($a(t) = 1$):**

$$ds^2 = -c^2 dt^2 + \frac{1}{1-kr^2} dr^2 + r^2 d\theta^2 + r^2 \sin^2 \theta d\phi^2$$

This represents a static (non-expanding) universe.

**Small Scales or Short Times:**

Locally (e.g., near Earth), the metric reduces to the Minkowski metric of special relativity:
$$ds^2 = -c^2 dt^2 + dx^2 + dy^2 + dz^2$$
