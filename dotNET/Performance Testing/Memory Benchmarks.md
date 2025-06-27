Memory usage pattern can effect performance
```csharp
public class Benchmarks
{
	private int n = 512;
	private long[,] a;
	
	[GlobalSetup]
	public void Setup()
	{
		a = new long[n, n];
	}
	
	[Benchmark(Baseline = true)]
	public long SumIj()
	{
		long sum = 0;
		for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
			sum += a[i, j];
		return sum;
	}
	
	[Benchmark]
	public long SumJi()
	{
		long sum = 0;
		for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
			sum += a[j, i];
		return sum;
	}
}
```
`SumIj` works faster because the array gets copied in the CPU cache; `SumJi` produces cache misses.

The algorithm also works faster when the array fits into the L1/L2 caches of the CPU.

