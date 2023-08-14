A Category ($C$) is a structure consisting of:
- Objects
- Morphisms
- Compositions
- Identities

## Objects

Objects are Elements in a Set.

The order is not relevant:

$$\{ 1, 2, 3 \} = \{ 3, 2, 1 \}$$

Repetition of an object is not relevant:

$$\{ 1, 1, 1 \} = \{ 1 \}$$

## Morphisms

A morphism is a relationship (transformation functions) between Set A to Set B.
A morphism might be one-way.

Morphisms can be visualized using a **commutative diagram**:
- [Wolfram MathWorld: Commutative Diagram](https://mathworld.wolfram.com/CommutativeDiagram.html)
- [Wikipedia: Commutative Diagram](https://en.wikipedia.org/wiki/Commutative_diagram)
- [Math3ma: Commutative Diagrams Explained](https://www.math3ma.com/blog/commutative-diagrams-explained)

## Compositions

Morphisms can be combined.

If there is a morphism $f : A \rightarrow B$ and a morphism $g : B \rightarrow C$ then there must be a morphism $$h = f ∘ g$$ $$h : A \rightarrow C$$

## Identities

A identity is a morphism from one object to itself.
The Identity-Morphism is required.

## Rules

Identity morphisms must follow **Identity-Law**, such that combining an Element with it's Element-Morphism returns the same Element:

$$I_e(e) = e$$

Morphisms must follow **Associativity-Law**:

$$(h∘g)∘f = h∘(g∘f) = h∘g∘f$$

If the identity law or the associativity law is not met, than the structure is not a category.