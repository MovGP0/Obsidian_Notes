A **Functor** is a map between two sets.

A functor $F : C \rightarrow D$ between two categories $C$ and $D$ consists of
- A Function on objects
- A Function on morphisms
- Must preserve identities, such that a identity of $C$ becomes an identity in $D$
- Must respect composition of morphisms, such that $F(g ∘ f) = F(g) ∘ F(f)$

## Forgetful Functor $U$

Removes one or more properties of the elements.

Ie. extracting the set of elements from a group using the $U: Grp \rightarrow Set$ functor.

## Free Group Functor

The **Free Group Functor** $F: Set \rightarrow Grp$ creates a [[Group]] from any set by adding the required properties.
