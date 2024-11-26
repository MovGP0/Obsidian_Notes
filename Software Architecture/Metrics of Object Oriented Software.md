## Lines of Code (LOC)

**Description**: The total number of lines in a program or function, including or excluding comments and blank lines.

**Calculation**:  
Count the number of lines, distinguishing between total lines, executable lines, and comments.

- **TLOC**: *Total Lines Of Code* that will counts non-blank and non-comment lines in a compilation unit. usefull for thoses interested in computed KLOC (*kilo Lines Of Code*).
- **MLOC**: *Method Lines Of Code* counts and sum non-blank and non-comment lines _inside_ method bodies

**Why It Matters**:

- Gives a rough estimate of the size of the codebase.
- Helps to gauge productivity and complexity (though not always a precise indicator).

## Number of Classes

Total number of classes in the selected scope

## Number of Children

Total number of **direct** subclasses of a class. A class implementing an interface counts as a direct child of that interface

## Number of Interfaces

Total number of interfaces in the selected scope

## Number of Methods (NOM)

Total number of methods defined in the selected scope

## Number of Overridden Methods (NORM)

Total number of methods in the selected scope that are overridden from an ancestor class

## Number of Fields

Total number of fields defined in the selected scope

## Specialization Index

Average of the specialization index, defined as 

$$SI = \frac{NORM * DIT}{NOM}$$

This is a class level metric

## Weighted Methods per Class (WMC)

Sum of the McCabe Cyclomatic Complexity for all methods in a class

## Lack of Cohesion of Methods (LCOM*)

A measure for the Cohesiveness of a class. Calculated with the Henderson-Sellers method (LCOM*).

If ($m(A)$ is the number of methods accessing an attribute $A$, calculate the average of $m(A)$ for all attributes, subtract the number of methods m and divide the result by $(1-m)$. A low value indicates a cohesive class and a value close to $1$ indicates a lack of cohesion and suggests the class might better be split into a number of (sub)classes.

This metric penalizes the proper use of getters and setters in Java, as the only methods that directly access an attribute and the other methods using the getter/setter methods.

## Cyclomatic Complexity (CC)

**Description**: Cyclomatic complexity measures the number of independent paths through the source code. It provides an indication of the code's complexity and helps assess how difficult the code is to test and maintain.

**Calculation**:

$$CC = E - N + 2P$$

Where:
- `E` = the number of edges in the control flow graph
- `N` = the number of nodes
- `P` = the number of connected components (typically 1 for a single function or method).

**Why It Matters**:

- Higher complexity increases the likelihood of bugs and maintenance difficulties.
- It guides developers to refactor overly complex methods.

**Example:**

```csharp
public void ProcessData(int value)
{
	if (value > 0) // Path 1
	{
		Console.WriteLine("Positive");
	}
	else if (value < 0) // Path 2
	{
	    Console.WriteLine("Negative");
	}
	else // Path 3
	{
		Console.WriteLine("Zero");
	}
}`
```

Cyclomatic complexity = 3 (three independent paths).

### McCabe Cyclomatic Complexity

Counts the number of flows through a piece of code. Each time a branch occurs (`if`, `for`, `while`, `do`, `case`, `catch` and the `?:` ternary operator, as well as the `&&` and `||` conditional logic operators in expressions) this metric is incremented by one.

For a full treatment of this metric see [McCabe](http://www.mccabe.com/nist/nist_pub.php).

### Halstead Metrics (V, D)

**Description**: Halstead metrics quantify complexity using the number of operators and operands.

**Key Measures**:

- $n_1$: Number of unique operators
- $n_2$: Number of unique operands
- $N_1$: Total occurrences of operators
- $N_2$: Total occurrences of operands

Key calculations:
- **Program Volume**: $V = \left( N_1 + N_2 \right) \cdot log⁡_{2}\left( n_1+n_2 \right)$
- **Program Difficulty**: $D=\frac{n_1}{2} \cdot \frac{N_2}{n_2}$

**Why It Matters**:

- Measures the cognitive effort needed to understand the code.
- Identifies overly complex modules.

## Code Coverage

**Description**: Measures the percentage of code executed during automated tests.

**Calculation**:

$$\text{Code\,Coverage} = \frac{ \text{Number\,of\,Covered\,Statements} }{ \text{Total\,Number\,of\,Statements} }$$

**Why It Matters**:

- Ensures critical parts of the code are tested.
- Improves software reliability by highlighting untested paths.

## Maintainability Index (MI)

**Description**: A composite metric based on cyclomatic complexity, LOC, and Halstead Volume. It provides an overall score of how maintainable the code is.

**Calculation** (simplified formula):  
- $\text{Maintainability\,Index}$ = $171 - 5.2 \cdot \ln ⁡\left( \text{Halstead\,Volume} \right)$ - $0.23\cdot \text{Cyclomatic\,Complexity}$ - $16.2 \cdot \ln \left(\text{LOC}\right)$

**Why It Matters**:

- A low score indicates code that is difficult to maintain, possibly requiring refactoring.

## Depth of Inheritance Tree (DIT)

**Description**: Measures the maximum depth of inheritance in a class hierarchy.

**Calculation**:
Count the levels of inheritance from a base class to a derived class.

**Why It Matters**:

- Deep inheritance hierarchies can increase complexity and reduce code clarity.

## Coupling (C)

**Description**: Measures the degree to which different classes or modules depend on each other.

- **Afferent Coupling (Ca)**: The number of classes or modules that depend on a given class/module.
- **Efferent Coupling (Ce)**: The number of external classes or modules that a given class/module depends on.

**Interpretation**
- **Low Coupling** Promotes reusability, easier testing, and better modularity.
- **High Coupling** Indicates a tightly bound system that is difficult to modify or test.

## Cohesion

**Description**: Measures how closely related the responsibilities of a class or module are.

**Calculation**: Analyze the relationships and usage of methods within a class.

**Why It Matters**:
- High cohesion improves readability, maintainability, and reusability.

**Example**:

```csharp
public class Calculator
{
	public int Add(int a, int b) => a + b;
	public int Subtract(int a, int b) => a - b;
}
```

High cohesion: All methods are arithmetic-related.

## Instability

Instability measures the balance between efferent and afferent coupling for a class or module. It quantifies how much a module depends on others versus how much others depend on it.

**Calculation**

$$I=\frac{Ce}{Ca+Ce}$$

Where:
- `Ce` = Efferent Coupling (outgoing dependencies)
- `Ca` = Afferent Coupling (incoming dependencies)

**Interpretation**
- $I = 0$: The module is completely stable (only dependent upon by others).
- $I = 1$: The module is completely unstable (depends only on others).
- $I \approx 0.5$: Balanced Instability. Values close to 0.5 indicate better modularity.

- Stable modules (low `I`) should be abstract and provide core functionality.
- Unstable modules (high `I`) can be concrete and depend on abstractions or other modules.

- **High Instability**:
    - **Pros**: Easier to change without affecting others.
    - **Cons**: Sensitive to changes in dependencies; may indicate high coupling.
- **Low Instability**:
    - **Pros**: Less sensitive to external changes; others rely on it.
    - **Cons**: Harder to change without impacting dependent modules.

### Abstractness

The number of abstract classes (and interfaces) divided by the total number of types in a package

$A = \frac{ N_A }{ N }$

### Normalized Distance from Main Sequence

$D_n = \left| A + I - 1 \right|$

This number should be small, close to zero for good packaging design.

## References

- [How to measure module coupling and instability using NDepend](https://codinghelmet.com/articles/how-to-measure-module-coupling-and-instability-using-ndepend)
- [.NET Code Metrics Values](https://learn.microsoft.com/en-us/visualstudio/code-quality/code-metrics-values)
- [Java Metrics](https://metrics.sourceforge.net/)
- [YouTube: The Sable Dependency Principle](https://www.youtube.com/watch?v=URa306q8mn8) by Uncle Bob

## Literature

- Agile Software Development, Robert C. Martin
- [OO Design Quality Metrics](https://linux.ime.usp.br/~joaomm/mac499/arquivos/referencias/oodmetrics.pdf)
