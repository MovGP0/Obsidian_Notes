```xml
<Project Sdk="Microsoft.NET.Sdk">  
  
	<PropertyGroup>  
		<TargetFramework>netstandard2.0</TargetFramework>  
		<Nullable>enable</Nullable>  
		<ImplicitUsings>enable</ImplicitUsings>  
		<ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
	</PropertyGroup>
  
	<ItemGroup>
		<PackageReference Include="Microsoft.CodeAnalysis.CSharp" />
		<PackageReference Include="Microsoft.CodeAnalysis.Analyzers" />
	<ItemGroup>

</Project>
```

## Implement `ISourceGenerator`

- implement `Microsoft.CodeAnalysis.ISourceGenerator`
- implement the `Initialize` method to register any required generator actions. Typically, you would use the `context.RegisterForSyntaxNotifications` method to register a syntax receiver
- implement the `Execute` method to generate the source code. This method will be called by the compiler with a `GeneratorExecutionContext` that provides access to the compilation, syntax trees, and other required information.

## Implement `ISyntaxReceiver`

Used to analyze the syntax tree of the code.

- implement `Microsoft.CodeAnalysis.ISyntaxReceiver` or `Microsoft.CodeAnalysis.ISyntaxContextReceiver`
- implement the `OnVisitSyntaxNode` or `OnVisitSyntaxNode` method to analyze the syntax nodes in the compilation and store any required information for later use in the generator's `Execute` method.

## Add code generator to project

Create `SourceGeneratorAttribute.cs`
```csharp
using System;

[AttributeUsage(AttributeTargets.Assembly, Inherited = false, AllowMultiple = true)]
internal sealed class SourceGeneratorAttribute : Attribute
{
	public SourceGeneratorAttribute(Type generatorType)
	{
		GeneratorType = generatorType;
	}
	public Type GeneratorType { get; }
}
```

Register generator using `AssemblyInfo.cs`
```csharp
using System.Runtime.CompilerServices;

[assembly: SourceGenerator(typeof(MyNamespace.MySourceGenerator))]
```

## Example using Plain Text

```csharp
using System;
using System.Text;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.Text;

[Generator]
public sealed class HelloWorldGenerator : ISourceGenerator
{
    public void Initialize(GeneratorInitializationContext context)
    {
        // No initialization required for this generator
    }

    public void Execute(GeneratorExecutionContext context)
    {
        // Define the generated code
        string source = @"
namespace GeneratedCode
{
    public static class HelloWorld
    {
        public static void SayHello()
        {
            System.Console.WriteLine(""Hello, World from generated code!"");
        }
    }
}";

        // Add the generated code to the compilation
        context.AddSource("HelloWorldGenerator", SourceText.From(source, Encoding.UTF8));
    }
}
```

## Example using a SyntaxTree

```csharp
using System;
using System.Linq;
using System.Text;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.CSharp.Syntax;
using Microsoft.CodeAnalysis.Text;

[Generator]
public class HelloWorldGenerator : ISourceGenerator
{
    public void Initialize(GeneratorInitializationContext context)
    {
        // No initialization required for this generator
    }

    public void Execute(GeneratorExecutionContext context)
    {
        // Define the generated code using a syntax tree
        var namespaceDeclaration = SyntaxFactory.NamespaceDeclaration(SyntaxFactory.ParseName("GeneratedCode"))
            .NormalizeWhitespace();

        var classDeclaration = SyntaxFactory.ClassDeclaration("HelloWorld")
            .AddModifiers(
	            SyntaxFactory.Token(SyntaxKind.PublicKeyword),
	            SyntaxFactory.Token(SyntaxKind.StaticKeyword))
            .NormalizeWhitespace();

        var methodDeclaration = SyntaxFactory.MethodDeclaration(SyntaxFactory.ParseTypeName("void"), "SayHello")
            .AddModifiers(SyntaxFactory.Token(SyntaxKind.PublicKeyword), SyntaxFactory.Token(SyntaxKind.StaticKeyword))
            .WithBody(SyntaxFactory.Block(
                SyntaxFactory.ExpressionStatement(
                    SyntaxFactory.InvocationExpression(
                        SyntaxFactory.MemberAccessExpression(
                            SyntaxKind.SimpleMemberAccessExpression,
                            SyntaxFactory.ParseTypeName("System.Console"),
                            SyntaxFactory.IdentifierName("WriteLine")
                        )
                    ).WithArgumentList(
                        SyntaxFactory.ArgumentList(
                            SyntaxFactory.SingletonSeparatedList(
                                SyntaxFactory.Argument(
	                                SyntaxFactory.LiteralExpression(
		                                SyntaxKind.StringLiteralExpression,
		                                SyntaxFactory.Literal("Hello, World from generated code!")))
                            )
                        )
                    )
                )
            ))
            .NormalizeWhitespace();

        classDeclaration = classDeclaration.AddMembers(methodDeclaration);
        namespaceDeclaration = namespaceDeclaration.AddMembers(classDeclaration);

        var compilationUnit = SyntaxFactory.CompilationUnit().AddMembers(namespaceDeclaration).NormalizeWhitespace();
        var generatedCode = compilationUnit.ToFullString();

        // Add the generated code to the compilation
        context.AddSource("HelloWorldGenerator", SourceText.From(generatedCode, Encoding.UTF8));
    }
}
```