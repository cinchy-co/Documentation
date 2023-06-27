# Extended Methods on Geometry & Geography Instances

## Overview

Cinchy CQL supports several extended methods on Open Geospatial Consortium (OGC) methods on geometry and geography instances.

{% hint style="warning" %}
All functions that have Geometry in parenthesis are only applicable to OGC methods on geometry instances.
{% endhint %}

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

The extended Methods covered in this section are:

<table data-header-hidden><thead><tr><th></th><th width="137"></th><th width="101"></th><th></th></tr></thead><tbody><tr><td></td><td></td><td></td><td></td></tr><tr><td><a href="extended-methods-on-geometry-and-geography-instances.md#isvaliddetailed-geometry">IsValidDetailed</a></td><td><a href="extended-methods-on-geometry-and-geography-instances.md#makevalid-geometry">MakeValid</a></td><td><a href="extended-methods-on-geometry-and-geography-instances.md#reduce-geometry">Reduce</a></td><td><a href="extended-methods-on-geometry-and-geography-instances.md#shortestlineto-geometry">ShortestLineTo</a></td></tr></tbody></table>

## IsValidDetailed (Geometry)

`IsValidDetailed()`returns a message that can help to identify problems with a spatial object that's not valid.

Only the first error is returned, when the object is not valid. When the object is valid, a value of 24400 is returned.

### Syntax

```
.IsValidDetailed()
```

### Return types

CQL: Text

### Remarks

The following table contains possible return values:

| Return Value | Description                                                                                                                    |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| 24400        | Valid                                                                                                                          |
| 24401        | Not valid, reason unknown.                                                                                                     |
| 24402        | Not valid because point {0} is an isolated point, which is not valid in this type of object.                                   |
| 24403        | Not valid because some pair of polygon edges overlap.                                                                          |
| 24404        | Not valid because polygon ring {0} intersects itself or some other ring.                                                       |
| 24405        | Not valid because some polygon ring intersects itself or some other ring.                                                      |
| 24406        | Not valid because curve {0} degenerates to a point.                                                                            |
| 24407        | Not valid because polygon ring {0} collapses to a line at point {1}.                                                           |
| 24408        | Not valid because polygon ring {0} is not closed.                                                                              |
| 24409        | Not valid because some portion of polygon ring {0} lies in the interior of a polygon.                                          |
| 24410        | Not valid because ring {0} is the first ring in a polygon of which it is not the exterior ring.                                |
| 24411        | Not valid because ring {0} lies outside the exterior ring {1} of its polygon.                                                  |
| 24412        | Not valid because the interior of a polygon with rings {0} and {1} is not connected.                                           |
| 24413        | Not valid because of two overlapping edges in curve {0}.                                                                       |
| 24414        | Not valid because an edge of curve {0} overlaps an edge of curve {1}.                                                          |
| 24415        | Not valid some polygon has an invalid ring structure.                                                                          |
| 24416        | Not valid because in curve {0} the edge that starts at point {1} is either a line or a degenerate arc with antipodal endpoints |

#### Example

This example of an invalid spatial object shows how the `IsValidDetailed()` methods behaves:

```sql
DECLARE @p GEOMETRY = 'Polygon((2 2, 4 4, 4 2, 2 4, 2 2))'
SELECT @p.IsValidDetailed()
--Returns: 24404: Not valid because polygon ring (1) intersects itself or some other ring.
```

## MakeValid (Geometry)

`MakeValid()`converts an invalid geometry instance into a geometry instance with a valid Open Geospatial Consortium (OGC) type.

### Syntax

```
.MakeValid ()
```

### Return Types

CQL: geometry

### Remarks

This method may cause a change in the type of the geometry instance, as well as cause the points of a geometry instance to shift slightly.

#### Example

This example creates an invalid `LineString` instance that overlaps itself and uses `MakeValid()` to make this instance valid:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 1 1, 1 0, 1 1, 2 2)', 0);
SET @g = @g.MakeValid();
```

## Reduce (Geometry)

By running the Douglas-Peucker algorithm on the instance with the given tolerance, `Reduce()`returns an approximation of the given geometry instance produced.

### Syntax

```
.Reduce ( tolerance )
```

### Arguments

_tolerance_\
The tolerance (type float) to input for the approximation algorithm.

#### Return types

CQL: geometry

### Remarks

This algorithm operates independently on each geometry contained in the instance, for collection types.

Does not modify `Point`instances.

For `CircularString`instances,`Reduce()` returns a `LineString`, `CircularString`, or `CompoundCurve` instance.

For `CompoundCurve`instances,`Reduce()` returns either a `CompoundCurve`or `LineString`instance.

On `Polygon`instances, the approximation algorithm is applied independently to each ring. If the returned `Polygon`instance is not valid, `Reduce()` will produce a `FormatException.`

When a circular arc segment is found, the approximation algorithm checks whether the arc can be approximated by its chord within half the given tolerance. Chords meeting this criteria have the circular arc replaced in the calculations by the chord. If a chord doesn't meet this criteria, then the circular arc is kept and the approximation algorithm is applied to the remaining segments.

#### Example

This example creates a `LineString` instance and uses `Reduce()` to simplify the instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 0 1, 1 0, 2 1, 3 0, 4 1)', 0);
SELECT @g.Reduce(.75).ToString();
```

## ShortestLineTo (Geometry)

`ShortestLineTo()`returns a `LineString`instance (which is the distance between the two geometry instances) with two points that represent the shortest distance between the two geometry instances.

### Syntax

```sql
.ShortestLineTo ( other_instance )
```

### Arguments

_other_instance_\
Specifies the second geometry instance that the calling geometry instance is trying to determine the shortest distance to.

### Return types

CQL: geometry

### Remarks

Returns a `LineString` instance with endpoints lying on the borders of the two non-intersecting geometry instances being compared.

The length of the `LineString`returned equals the shortest distance between the two geometry instances.

Returns an empty `LineString`instance when the two geometry instances intersect each other.

#### Example

This example returns the `LineString` instance connecting the two points, by finding the shortest distance between a `CircularString` instance and a `LineString` instance:

```sql
 DECLARE @g1 geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';
 DECLARE @g2 geometry = 'LINESTRING(-4 7, 7 10, 3 7)';
 SELECT @g1.ShortestLineTo(@g2).ToString();
```
