## Create Skill with Semantic Functions

```csharp
using Microsoft.SemanticKernel.SkillDefinition;
```

```csharp
public sealed class MySkill
{
    [SKFunction("DESCRIPTION WHAT THE FUNCTION DOES")]  
	public string Foo()  
	{
	    return ...
	}

	[SKFunction("DESCRIPTION WHAT THE FUNCTION DOES")]
	[SKFunctionName("Foo")] // override function name (Optional)
	[SKFunctionInput(Description = "DESCRIPTION OF THE INPUT PARAMETER")]
	public string Bar(string input)
	{
		return ...
	}

	[SKFunction("DESCRIPTION WHAT THE FUNCTION DOES")]
	[SKFunctionName("Bar")] // override function name (Optional)
	[SKFunctionInput(Description = "DESCRIPTION OF THE INPUT PARAMETER")]
	[SKFunctionContextParameter(Name = "key", Description = "DESCRIPTION OF THE VARIABLE", DefaultValue = "")]
	public async Task<string> Baz(string input, SKContext context)
	{
		var hasKey = context.Variables.Get("key", out string @value);
		if(hasKey)
		{
			var variables = new ContextVariables(input);
			variables.Set("key", newValue);
		}
		return ...
	}

    // TODO: set in Constructor
	private readonly ISKFunction _helperFunction;

    [SKFunction("DESCRIPTION WHAT THE FUNCTION DOES")]
	[SKFunctionName("Bar")] // override function name (Optional)
	[SKFunctionInput(Description = "DESCRIPTION OF THE INPUT PARAMETER")]
	[SKFunctionContextParameter(Name = "key", Description = "DESCRIPTION OF THE VARIABLE", DefaultValue = "")]
    public async Task<SkContext> Qux(string input, SKContext context)
    {
	    try
	    {
	       var innerContext = new SKContext(
		       bucketVariables,
		       context.Memory,
		       context.Skills,
		       context.Log,
		       context.CancellationToken);
			var result = await _helperFunction.InvokeAsync(innerContext);
			var resultString = result.Result;

		    return context;
	    }
	    catch(Exception e)
	    {
	        context.Log(e.Message);
		    return context.Fail(e.Message);
	    }
    }

    // TODO: add additional functions as needed
}
```

Load Skill using `kernel.ImportSkill`
```csharp
var mySkill = new MySkill();
var skill = kernel.ImportSkill(mySkill);
ISKFunction foo = skill[nameof(MySkill.Foo)];
```
```csharp
ISKFunction bar = kernel.ImportSkill(new MySkill(), nameof(MySkill.Bar));
```

## Create Semantic Function using `CreateSemanticFunction`

```csharp
ISKFunction qux = kernel.CreateSemanticFunction(
	promptTemplate: "foobar {{$input}}", 
	skillName: "NAME", 
	description: "DESCRIPTION",
	maxTokens: 1024,
	temperature: 0.1,
	topP: 0.5);
```

## Create Semantic Function from Template File

- create a folder named after the skill
- Create a sub-folder for each semantic function
- create the files `skpromt.txt` and `config.json` in the folder for the semantic function

`skpromt.txt` contains the template for the promt
```txt
The time is {{$time}}.
```

`config.json` contains the prompt settings
```json
{
  "schema": 1,
  "description": "Gives the current time",
  "type": "completion",
  "completion": {
    "max_tokens": 150,
    "temperature": 0.9,
    "top_p": 0.0,
    "presence_penalty": 0.6,
    "frequency_penalty": 0.0,
    "stop_sequences": [
      "[Done]",
      "..."
    ]
  }
}
```

Load Semantic Function from folder
```csharp
ISKFunction function = kernel.ImportSemanticSkillFromDirectory(@"c:\path-to-skill\", "function_name");
```

Load all Semantic Functions from folder
```csharp
IDictionary<string, ISKFunction> allFunctions = kernel.ImportSemanticSkillFromDirectory(@"c:\path-to-skill\");
```

## Load ChatGPT Plugin

```csharp
var skill = await kernel.ImportChatGptPluginSkillFromUrlAsync("<skill name>", new Uri("<chatGPT-plugin>"));
```

## Execute Query with Semantic Functions

```csharp
using Microsoft.SemanticKernel.Orchestration;
```

```csharp
var variables = new ContextVariables("some user promt {{$key}}");
variables.Set("key", "value");

SKContext result = kernel.RunAsync(variables, foo, bar, baz);
Console.WriteLine(result);
```
