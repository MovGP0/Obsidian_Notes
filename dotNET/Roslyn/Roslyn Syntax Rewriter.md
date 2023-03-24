- add `Microsoft.CodeAnalysis.CSharp`
- Implement `CSharpSyntaxRewriter` class

## Example: multiply all `int` values by 2

Implement rewriter
```csharp
using System;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.CSharp.Syntax;

public class DoubleIntegerLiteralsRewriter : CSharpSyntaxRewriter
{
    public override SyntaxNode VisitLiteralExpression(LiteralExpressionSyntax node)
    {
        // Check if the node is an integer literal
        if (node.IsKind(SyntaxKind.NumericLiteralExpression) &&
            node.Token.Value is int intValue)
        {
            // Create a new integer literal with the doubled value
            var newNode = SyntaxFactory.LiteralExpression(
                SyntaxKind.NumericLiteralExpression,
                SyntaxFactory.Literal(intValue * 2)
            );
            
            return newNode;
        }

        // If not an integer literal, continue with the normal visitation process
        return base.VisitLiteralExpression(node);
    }
}
```

Execute rewriter
```csharp
using System;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;

class Program
{
    static void Main(string[] args)
    {
        string code = @"
using System;

namespace Demo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(3 + 4);
        }
    }
}";

        // Parse the source code into a syntax tree
        var tree = CSharpSyntaxTree.ParseText(code);
        var root = tree.GetCompilationUnitRoot();

        // Create an instance of the rewriter
        var rewriter = new DoubleIntegerLiteralsRewriter();

        // Visit and rewrite the syntax tree
        var newRoot = rewriter.Visit(root);

        // Print the modified code
        Console.WriteLine(newRoot.ToFullString());
    }
}
```

## Example: sort methods alphabetically

```csharp
using System.Collections.Generic;
using System.Linq;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.CSharp.Syntax;

public class MethodSortingRewriter : CSharpSyntaxRewriter
{
    public override SyntaxNode VisitClassDeclaration(ClassDeclarationSyntax node)
    {
        // Extract methods from the class
        var methods = node.Members.OfType<MethodDeclarationSyntax>().ToList();

        // Sort methods alphabetically by their identifier names
        var sortedMethods = methods.OrderBy(m => m.Identifier.Text).ToList();

        // Replace original methods with sorted methods
        node = node.ReplaceNodes(methods, (original, rewritten) => sortedMethods[methods.IndexOf(original)]);

        return base.VisitClassDeclaration(node);
    }
}
```

Usage
```csharp
using System;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;

class Program
{
    static void Main(string[] args)
    {
        string code = @"
using System;

namespace Demo
{
    class Program
    {
        public void Zeta() { }
        public void Beta() { }
        public void Alpha() { }
    }
}";

        // Parse the source code into a syntax tree
        var tree = CSharpSyntaxTree.ParseText(code);
        var root = tree.GetCompilationUnitRoot();

        // Create an instance of the rewriter
        var rewriter = new MethodSortingRewriter();

        // Visit and rewrite the syntax tree
        var newRoot = rewriter.Visit(root);

        // Print the modified code
        Console.WriteLine(newRoot.ToFullString());
    }
}
```

## Implement MSBuild target to use the Syntax Rewriter on build

Implement `.targets` file
```xml
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <UsingTask
    TaskName="RewriteSyntaxOnBuild"
    AssemblyFile="$(MSBuildThisFileDirectory)YourRewriterProject.dll" />
  
  <Target Name="RewriteSyntax" BeforeTargets="CoreCompile">
    <ItemGroup>
      <Compile Include="@(SyntaxRewriterOutput)" />
    </ItemGroup>

    <RewriteSyntaxOnBuild
      Sources="@(Compile)"
      OutputDirectory="$(IntermediateOutputPath)" />
  </Target>
</Project>
```

Implement Target method
```csharp
using System;
using System.IO;
using System.Linq;
using Microsoft.Build.Framework;
using Microsoft.Build.Utilities;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;

public sealed class RewriteSyntaxOnBuild : Task
{
    [Required]
    public ITaskItem[] Sources { get; set; }

    [Required]
    public string OutputDirectory { get; set; }

    public override bool Execute()
    {
        var rewriter = new MethodSortingRewriter();

        foreach (var source in Sources)
        {
            var sourceFile = source.ItemSpec;
            var syntaxTree = CSharpSyntaxTree.ParseText(File.ReadAllText(sourceFile));
            var newRoot = rewriter.Visit(syntaxTree.GetRoot());

            // Save the rewritten syntax tree to the intermediate output path
            var newFile = Path.Combine(OutputDirectory, Path.GetFileName(sourceFile));
            File.WriteAllText(newFile, newRoot.ToFullString());
        }

        return true;
    }
}
```

Add the `.targets` file in the `.csproj` file to include in the NuGet package
```csharp
<ItemGroup>
  <None Include="MyProject.targets" Pack="true" PackagePath="build" />
</ItemGroup>
```
