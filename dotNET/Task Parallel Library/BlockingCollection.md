
## Example

```csharp
using System;
using System.Collections.Concurrent;
using System.Threading.Tasks;

public class Program
{
    public static void Main()
    {
        // Create the pipeline stages
        BlockingCollection<int> stage1 = new BlockingCollection<int>();
        BlockingCollection<int> stage2 = new BlockingCollection<int>();

        // Stage 1: Generate some data
        Task.Factory.StartNew(() =>
        {
            for (int i = 0; i < 10; i++)
            {
                stage1.Add(i);
                Console.WriteLine("Stage 1 produces: " + i);
            }
            stage1.CompleteAdding();
        });

        // Stage 2: Process the data
        Task.Factory.StartNew(() =>
        {
            foreach (int item in stage1.GetConsumingEnumerable())
            {
                int result = Process(item);
                stage2.Add(result);
                Console.WriteLine("Stage 2 produces: " + result);
            }
            stage2.CompleteAdding();
        });

        // Stage 3: Consume the results
        Task.Factory.StartNew(() =>
        {
            foreach (int item in stage2.GetConsumingEnumerable())
            {
                Console.WriteLine("Stage 3 consumes: " + item);
            }
        }).Wait(); // Wait for the final stage to complete
    }

    private static int Process(int item)
    {
        // Simulate some time-consuming work
        return item * item;
    }
}
```
