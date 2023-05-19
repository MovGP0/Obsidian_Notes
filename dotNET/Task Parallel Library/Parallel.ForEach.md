## Example

```csharp
using System;
using System.Threading.Tasks;

public class Program
{
    public static void Main()
    {
        // Create a large data collection
        int[] data = new int[1000000];
        for (int i = 0; i < data.Length; i++)
        {
            data[i] = i;
        }

        // Use Parallel.ForEach to process the data in parallel
        Parallel.ForEach(data, item =>
        {
            // Perform some operation on the item
            Process(item);
        });
    }

    private static void Process(int item)
    {
        // Simulate some time-consuming work
        double result = Math.Sqrt(item);
    }
}
```