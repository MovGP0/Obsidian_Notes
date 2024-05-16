Represents a NURBS Spline

### Common parameters

Indicates the start of a spline entity.

```d
0
SPLINE
```

The handle for this entity.

```d
5
23D
```

The start of the common entity data.

```d
100
AcDbEntity
```

Layer name/number.

```d
8
0
```

Subclass marker: Marks the start of the spline-specific data.

```d
100
AcDbSpline
```

### Color

Color number, which is set to 0 (BYLAYER).

```d
62
0
```

### Line type

Line type, which is set to "BYLAYER".

```d
6
BYLAYER
```

### Spline flags

Binary flags for the type of spline.

| Flag | Description                     |
| ---- | ------------------------------- |
| 0x1  | Closed spline                   |
| 0x2  | Periodic spline                 |
| 0x4  | Rational spline                 |
| 0x8  | Planar                          |
| 0xF  | Linear (planar bit is also set) |

```d
70
0
```

### Degree

Degree of the spline

| Value | Description      |
| ----- | ---------------- |
| 2     | Quadratic spline |
| 3     | Cubic spline     |

```d
71
3
```

### Meta information

| ID  | Description              |
| --- | ------------------------ |
| 72  | Number of Knots          |
| 73  | Number of Control points |
| 74  | Number of Fit points     |

```d
72
34
73
30
74
28
```

### Tolerances

| Id  | Description                |
| --- | -------------------------- |
| 42  | Tolerance of fitting       |
| 43  | Tolerance of start tangent |
| 44  | Tolerance of end tangent   |

```d
42
0.0000001000
43
0.0000001000
44
0.0000000001
```

### Start tangent vector

```d
12
91.2088523876
22
0.0044824063
32
0.0000000000
```

### End tangent vector

```d
13
0.0044824063
23
91.2088523876
33
0.0000000000
```

### Knots

Knot vector values that influence the parameterization of the spline.
There are 4 more knots than the number of control points.

```d
40
0.0000000000
```

### Control Points

The coordinates (X, Y, Z) of the control points that shape the spline.

| Id  | Description                       |
| --- | --------------------------------- |
| 10  | X-Coordinate of the control point |
| 20  | Y-Coordinate of the control point |
| 30  | Z-Coordinate of the control point |

```d
10
1205.3151065802
20
447.6628651080
30
0.0000000000
```

### Fit Points

The coordinates (X, Y, Z) of the fit points through which the spline will pass.
There are 2 less fit points than the number of control points.

| Id  | Description                   |
| --- | ----------------------------- |
| 11  | X-Coordinate of the fit point |
| 21  | Y-Coordinate of the fit point |
| 31  | Z-Coordinate of the fit point |

```d
11
1205.3151065802
21
447.6628651080
31
0.0000000000
```

## Documentation

- [SPLINE (DXF)](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-E1F884F8-AA90-4864-A215-3182D47A9C74)
