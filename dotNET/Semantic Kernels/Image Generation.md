Create Kernel
```csharp
var kernel = Microsoft.SemanticKernel.Kernel.Builder
    .Configure(c =>
    {
        c.AddOpenAIEmbeddingGenerationService("ada", "text-embedding-ada-002", apiKey);
        c.AddOpenAITextCompletionService("davinci", "text-davinci-003", apiKey, orgId);
        c.AddOpenAIImageGenerationService("dallE", apiKey, orgId);
    })
    .Build();
```

Get image generator service
```csharp
var imageGenerator = kernel.GetService<IImageGeneration>();
```

Get text embedding service
```csharp
var textEmbedding = kernel.GetService<IEmbeddingGeneration<string, float>>();
```

Create image generation function
```csharp
var promtGenerator = kernel.CreateSemanticFunction(
	"{{$input}}, unreal engine, UHD, 8k",
	maxTokens: 256,
	temperature: 1);
```

Create a prompt
```csharp
var promt = await promtGenerator.InvokeAsync("super mario");
```

Generate image and return URL
```csharp
var imageUrl = await imageGenerator.GenerateImageAsync(imageDescription.Result.Trim(), 512, 512);
```
