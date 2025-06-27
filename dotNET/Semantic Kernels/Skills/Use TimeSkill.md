```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.CoreSkills;
using Microsoft.SemanticKernel.KernelExtensions;

IKernel kernel = Kernel.Builder.Build();

kernel.Config.AddOpenAITextCompletionService("davinci", "text-davinci-003", apiKey, "");

kernel.ImportSkill(new TimeSkill(), "time");
```

```csharp
var definition = """
Today is: {{time.Date}}  
Current time is: {{time.Time}}

{{$input}}
""";
```

```csharp
var kindOfDay = kernel.CreateSemanticFunction(definition, maxTokens: 150);  

var result = await kindOfDay.InvokeAsync(input);  
Console.WriteLine(result);
```