
Things to consider in regards to CPU performance:
- Registers and Stack
- JIT Inlining
- Instruction-Level Parallelism
- Branch Prediction
- Arithmetic Operations (ie. float vs. double)
- Intrinsics (JIT creates CPU-specific code)

## JIT Inlining

Advantages
- Eliminates call overhead
- Enables further optimization techniques
- Sometimes makes register allocation better

Disadvantages
- Increased file size
- Might inline the wrong method 
	- ie. given `A(B(C()))` and JIT inlines to `A(BC())` but `AB(C())` would be more efficient
- Sometimes makes register allocation worse

## Intrinics

Intrinsics are used implicitly by the JIT compiler in many cases.

Use `System.Runtime.Intrinsics` to check if the CPU supports secific intrinsics and use them explicitly

Prefer SIMD optimized types in `System.Numerics` like `Vector4` and `Matrix4x4`.

### Examples

Use SSE to count the number of 1 bits of a value 
```csharp
if(Popcnt.IsSupported)
{
	return Popcnt.PopCount(x); // use SSE4 method
}
else
{
	// custom implementation
}
```

Use SSE/AVX to compare `int[]`
```csharp
public static bool NotEqualManual(int[] x, int[] y) 
{ 
	if(x.Length != y.Length) return false;
	for (int i = 0; i < x.Length; i++)
		if (x[i] == y[i]) 
			return false;
	return true;
}

public static bool NotEqualSse41(int[] x, int[] y)
{
	if(x.Length != y.Length) return false;
	if(x.Length % 4 != 0) throw new NotSupportedException();

	fixed (int* xp = &x[0])
	fixed (int* yp = &y[0])
	{
		for (int i = 0; i < x.Length; i += 4)
		{
			Vector128<int> xVector = Sse2.LoadVector128(xp + i);
			Vector128<int> yVector = Sse2.LoadVector128(yp + i);
			Vector128<int> mask = Sse2.CompareEqual(xVector, yVector);
			
			if (!Sse41.TestAllZeros(mask, mask)) return false;
		}
	}
	return true;
}

public static bool NotEqualAvx2(int[] x, int[] y)
{
	if(x.Length != y.Length) return false;
	if(x.Length % 8 != 0) throw new NotSupportedException();

	fixed (int* xp = &x[0])
	fixed (int* yp = &y[0])
	{
		for (int i = 0; i < x.Length; i += 8)
		{
			Vector256<int> xVector = Avx.LoadVector256(xp + i);
			Vector256<int> yVector = Avx.LoadVector256(yp + i);
			Vector256<int> mask = Avx2.CompareEqual(xVector, yVector);
			
			if (!Avx.TestZ(mask, mask)) return false;
		}
	}
	return true;
}
```

