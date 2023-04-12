## Using the Planner

Create a new kernel
```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.KernelExtensions;

IKernel kernel = Kernel.Builder.Build();
```

Configure the AI backend
```csharp
kernel.Config.AddOpenAITextCompletionService("davinci", "text-davinci-003", apiKey, orgId);
```

Add the planner skill to the kernel
```csharp
using Microsoft.SemanticKernel.CoreSkills;

var planner = kernel.ImportSkill(new PlannerSkill(kernel, maxTokens: 1024));
```

Add custom skills
```csharp
using Microsoft.SemanticKernel.Orchestration;

kernel.ImportSkill(...);
kernel.CreateSemanticFunction(...);
kernel.ImportSemanticSkillFromDirectory(...);
```

Create plan using the `CreatePlan` skill
```csharp
var originalPlan = await kernel.RunAsync(query, planner["CreatePlan"]);
```

View plan XML
```csharp
Console.WriteLine(newPlan.Variables.ToPlan().PlanString);
```

Iterate plans to get to solution
```csharp
int step = 1;
const int MaxSteps = 10;

while (!executionResults.Variables.ToPlan().IsComplete && step < MaxSteps)
{
    var results = await kernel.RunAsync(executionResults.Variables, planner["ExecutePlan"]);
    if (results.Variables.ToPlan().IsSuccessful)
    {
        Console.WriteLine($"Step {step} - Execution results:\n");
        Console.WriteLine(results.Variables.ToPlan().PlanString);

        if (results.Variables.ToPlan().IsComplete)
        {
            Console.WriteLine($"Step {step} - COMPLETE!");
            Console.WriteLine(results.Variables.ToPlan().Result);
            break;
        }
    }
    else
    {
        Console.WriteLine($"Step {step} - Execution failed:");
        Console.WriteLine(results.Variables.ToPlan().Result);
        break;
    }
    executionResults = results;
    step++;
    Console.WriteLine("");
}
```

## How the planner works

- `BucketFunction` takes a list of items and buckets them into a number of buckets
- `FunctionFlowFunction` takes a goal and creates an XML plan that can be executed
- `FunctionFlowRunner` executes an XML plan step-by-step

## References

- [[Create Semantic Kernel]]