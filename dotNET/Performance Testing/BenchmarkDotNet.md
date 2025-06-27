```csharp
using System;
using System.Security.Cryptography;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

namespace MyBenchmarks;

[ClrJob(baseline: true), CoreJob, MonoJob, CoreRtJob]
public sealed class Md5VsSha256
{
	private SHA256 sha256 = SHA256.Create();
	private MD5 md5 = MD5.Create();
	private byte[] data;
	
	[Params(1000, 10000)]
	public int N;
	
	[GlobalSetup]
	public void Setup()
	{
		data = new byte[N];
		new Random(42).NextBytes(data);
	} 

	[Benchmark]
	public byte[] Sha256() => sha256.ComputeHash(data);

	[Benchmark]
	public byte[] Md5() => md5.ComputeHash(data);
}

public static class Program
{
	public static void Main(string[] args)
	{
		var summary = BenchmarkRunner.Run<Md5VsSha256>();
	}
}
```