## Create Kernel
```csharp
IKernel kernel = KernelBuilder.Create();

kernel.Config.AddAzureOpenAIEmbeddingGenerationService(
			"myName2",
			"embeddingsDeploymentName",
			"https://...",
			"AZURE_API_KEY",
			"2022-12-01");

kernel.Config.AddAzureOpenAITextCompletionService(
			"azure-text-davinci-002",
			"text-davinci-002",
			"https://...",
			"AZURE_API_KEY",
			"2022-12-01");

kernel.Config.AddOpenAITextCompletionService("text-davinci-002", "text-davinci-002", "OPENAI_API_KEY");

kernel.Config.SetDefaultTextCompletionService("text-davinci-002");

kernel.Log = ...;

kernel.Memory = ...;

// ...
```

## Create Kernel using Builder
```csharp
IKernel kernel = Kernel.Builder
	.WithLogger(NullLogger.Instance)  
	.WithMemory(memory)
	.Configure(c =>
	{
		c.AddAzureOpenAITextCompletionService(
			"myName1",
			"completionDeploymentName",
			"https://...",
			"apiKey",
			"2022-12-01");

		c.AddAzureOpenAIEmbeddingGenerationService(
			"myName2",
			"embeddingsDeploymentName",
			"https://...",
			"apiKey",
			"2022-12-01");
	})
	.Configure(c =>
	{
		// additional configuration
	})
	.WithMemoryStorageAndTextEmbeddingGeneration(memoryStorage, textEmbeddingGenerator)
	.WithRetryHandlerFactory(new RetryThreeTimesFactory())
	.Build();
```

## Create Kernel using constructor
```csharp
ILogger logger = ...;  

var skills = new SkillCollection();  

var memoryStorage = new VolatileMemoryStore();  

var textEmbeddingGenerator = new AzureTextEmbeddingGeneration(
	"modelId", "https://...", "apiKey", "2022-12-01", logger);  

var templateEngine = new PromptTemplateEngine(logger);  

var memory = new SemanticTextMemory(memoryStorage, textEmbeddingGenerator);  

ITextCompletion Factory(IKernel kernel)
{
    var retryConfig = new HttpRetryConfig();
    var httpHandlerFactory = new DefaultHttpRetryHandlerFactory(retryConfig);
	return new AzureTextCompletion("deploymentName", "https://...", "apiKey", "2022-12-01",
		logger,
		httpHandlerFactory); 
}

var config = new KernelConfig();
config.AddTextCompletionService("foo", Factory);

IKernel kernel = new Kernel(skills, templateEngine, memory, config, logger);
```
