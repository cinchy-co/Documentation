# OGC Methods on Geometry & Geography Instances

## Overview

Cinchy CQL supports the following on Open Geospatial Consortium (OGC) methods on geometry and geography instances.

{% hint style="warning" %}
All functions that have Geometry in parenthesis are only applicable to OGC methods on geometry instances.
{% endhint %}

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

The OGC Methods covered in this section are:

<table data-header-hidden><thead><tr><th width="153"></th><th width="126"></th><th width="141"></th><th></th><th></th></tr></thead><tbody><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#starea">STArea</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stasbinary">STAsBinary</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stastext">STAsText</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stboundary-geometry">STBoundary</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stbuffer">STBuffer</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stcentroid">STCentroid</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stcontains-geometry">STContains</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stconvexhull-geometry">STConvexHull</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stcrosses-geometry">STCrosses</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stcurvetoline-geometry">STCurveToLine</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stdifference-geometry">STDifference</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stdisjoint-geometry">STDisjoint</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stdistance">STDistance</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stendpoint-geometry">STEndpoint</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stenvelope-geometry">STEnvelope</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stequals-geometry">STEquals</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stexteriorring-geometry">STExteriorRing</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stgeometryn-geometry">STGeometryN</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stgeometrytype-geometry">STGeometryType</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stgeomcollfromtext-geometry">STGeomCollFromText</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stgeomfromtext">STGeomFromText</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stgeomfromwkb">STGeomFromWKB</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stinteriorringn-geometry">STInteriorRingN</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stintersection">STIntersection</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stintersects">STIntersects</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stisclosed-geometry">STIsClosed</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stisempty-geometry">STIsEmpty</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stisring-geometry">STIsRing</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stisring-geometry">STIsSimple</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stisvalid-geometry">STIsValid</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stlength">STLength</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stlinefromtext-geometry">STLineFromText</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stlinefromwkb-geometry">STLineFromWKB</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stmlinefromtext-geometry">STMLineFromText</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stmpointfromtext-geometry">STMPointFromText</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stmpolyfromtext-geometry">STMPolyFromText</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stnumcurves-geometry">STNumCurves</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stnumgeometries-geometry">STNumGeometries</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stnuminteriorring-geometry">STNumInteriorRing</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stnumpoints-geometry">STNumPoints</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stoverlaps-geometry">STOverlaps</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stpointfromtext-geometry">STPointFromText</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stpointfromwkb-geometry">STPointFromWKB</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stpointn-geometry">STPointN</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stpointonsurface-geometry">STPointOnSurface</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stpolyfromtext-geometry">STPolyFromText</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#strelate-geometry">STRelate</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#ststartpoint-geometry">STStartPoint</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stsymdifference-geometry">STSymDifference</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#sttouches-geometry">STTouches</a></td></tr><tr><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stunion-geometry">STUnion</a></td><td><a href="ogc-methods-on-geometry-and-geography-instances.md#stwithin-geometry">STWithin</a></td><td></td><td></td><td></td></tr></tbody></table>

## STArea

`STArea()` returns the total surface area of a geometry/geography instance.

#### Syntax

```sql
.STArea ( )
```

#### Return Types

CQL: Number

#### Remarks

When the geometry/geography instance contains only zero- and one-dimensional figures, or if it's empty, `STArea()` returns 0.

#### Geometry Example

This example creates a `Polygon`geometry instance and computes the area of the `Polygon:`

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);
SELECT @g.STArea();
```

#### Geography Example

This example creates a `Polygon` geography instance and computes the area of the `Polygon`:

```sql
DECLARE @g geography;
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);
SELECT @g.STArea();
```

## STAsBinary

`STAsBinary()` returns the Open Geospatial Consortium (OGC) Well-Known Binary (WKB) representation of a geometry/geography instance.

#### Syntax

```sql
.STAsBinary ( )
```

#### Return Types

CQL: Base64 Text

#### Geometry Example

This example creates a `LineString` geometry instance from (0,0) to (2,3) from text. `STAsBinary()` returns the result in WKB:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 3)', 0);
SELECT @g.STAsBinary();
```

#### Geography Example

This example uses `STAsBinary()` to create a `LineString` geography instance from (-122.360, 47.656) to (-122.343, 47.656) from text. It then returns the result in WKB:

```sql
DECLARE @g geography;
SET @g = geography::STGeomFromText('LINESTRING( -122.360 47.656, -122.343 47.656)', 4326);
SELECT @g.STAsBinary();
```

## STAsText

`STAsText()` returns the Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation of a geometry/geography instance.&#x20;

#### Syntax

```sql
.STAsText ( )
```

#### Return Types

CQL: Text

#### Remarks

OGC type of a geography instance can be determined by invoking [`STGeometryType()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STGeometryType)

The return text will not contain`Z` (elevation) and `M` (measure) values carried by the instance.

#### Geometry Example

This example creates a `LineString` geometry instance from (0,0) to (2,3) from text. `STAsText()` returns the result in text:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 3)', 0);
SELECT @g.STAsText();
```

#### Geography Example

This example uses `STAsText()` to create a `LineString` geography instance from (-122.360, 47.656) to (-122.343, 47.656) from text. It then returns the result in text:

```sql
DECLARE @g geography;
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);
SELECT @g.STAsText();
```

## STBoundary (Geometry)

&#x20;`STBoundary()` returns the boundary of a geometry instance.

#### Syntax

```sql
.STBoundary ( )
```

#### Return Types

CQL: geometry

#### Geometry Example

This example uses `STBoundary()` on a `CurvePolygon` instance. `STBoundary()` returns a `CircularString` instance:

```sql
 DECLARE @g geometry;
 SET @g = geometry::STGeomFromText('CURVEPOLYGON(CIRCULARSTRING(0 0, 2 2, 0 2, -2 2, 0 0))', 0);
 SELECT @g.STBoundary().ToString();
```

## STBuffer

`STBuffer()`returns a geometric/geography object that represents the union of all points whose distance from a geometry/geography instance is less than or equal to a specified value.

#### Syntax

```sql
.STBuffer ( distance )
```

#### Arguments

_distance_\
A value of type float (double in the .NET Framework) specifying the distance from the geometry/geography instance around which to calculate the buffer.

#### Return Types

CQL: geometry/geography

#### Remarks

`STBuffer()` calculates a buffer specifying _tolerance_ = distance \* .001 and _relative_ = false.

A negative buffer removes all points within the given distance of the boundary of the geometry/geography instance.

The error between the theoretical and computed buffer is max(tolerance, extents _1.E-7) where tolerance = distance \*_ .001.

Geometry:&#x20;

When _distance_ > 0 then either a `Polygon`or `MultiPolygon`instance is returned. When _distance_ = 0, then a copy of the calling geometry instance is returned. When _distance_ < 0, then:

- When the dimensions of the instance are 0 or 1, an empty `GeometryCollection`instance is returned.
- when the dimensions of the instance are 2 or more, a negative buffer is returned.

Geography:

`STBuffer()` will return a `FullGlobe`instance in certain cases; for example, `STBuffer()` returns a `FullGlobe`instance when the buffer distance is greater than the distance from the equator to the poles. A buffer cannot exceed the full globe.

This method will throw an `ArgumentException`in `FullGlobe`instances where the distance of the buffer exceeds the following limitation: 0.999 \* _π_ \* minorAxis \* minorAxis / majorAxis (\~0.999 \* 1/2 Earth's circumference).

#### Geometry Example

&#x20;This example returns a `Polygon` instance with a negative buffer from a `CurvePolygon` instance:

```sql
DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))';
 SELECT @g.STBuffer(-1).ToString();
```

#### Geography Example

This example creates a `LineString` geography instance. It then uses `STBuffer()` to return the region within 1 meter of the instance:

```sql
DECLARE @g geography;
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);
SELECT @g.STBuffer(1).ToString();
```

## STCentroid

`STCentroid()` returns the geometry/geography center of a geometry/geography instance that consists of one or more `Polygons`.

#### Syntax

```sql
.STCentroid ( )
```

#### Return Types

CQL: geometry/geography

#### Remarks

If the geometry/geography instance is not a `Polygon`, `CurvePolygon`, or `MultiPolygon`type`STCentroid()` returns null.

#### Geometry Example

This example uses `STCentroid()` to compute the centroid of a `polygon`geography instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);
SELECT @g.STCentroid().ToString();
```

## STContains (Geometry)

`STContains()`returns 1 if a geometry instance completely contains another geometry instance, returns 0 if it does not.

#### Syntax

```sql
.STContains ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STContains()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

If the spatial reference IDs (SRIDs) of the geometry instances do not match, `STContains()` always returns null.

#### Geometry Example

This example uses `STContains()` to test two geometry instances to see if the first instance contains the second instance:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 2 0, 2 2, 0 2, 0 0))', 0);
SET @h = geometry::STGeomFromText('POINT(1 1)', 0);
SELECT @g.STContains(@h);
```

## STConvexHull (Geometry)

`STConvexHull()` returns an object representing the convex hull of a geometry instance.

#### Syntax

```sql
.STConvexHull ( )
```

#### Return Types

CQL: geometry

#### Remarks

Points or co-linear `LineString`instances will produce an instance of the same type as that of the input. `STConvexHull()` returns the smallest convex `Polygon`that contains the given geometry instance.

#### Geometry Example

This example uses `STConvexHull()` to find the convex hull of a non-convex `Polygon` geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 0 2, 1 1, 2 2, 2 0, 0 0))', 0);
SELECT @g.STConvexHull().ToString();
```

## STCrosses (Geometry)

`STCrosses()` returns 1 if a geometry instance crosses another geometry instance. Returns 0 if it does not.

#### Syntax&#x20;

```sql
.STCrosses ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry/geography instance to compare against the instance on which `STCrosses()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

If the spatial reference IDs (SRIDs) of the geometry instances do not match, this method always returns null.

Both conditions must be true for two geometry instances to cross:

- The intersection of the two geometry instances results in a geometry whose dimensions are less than the maximum dimension of the source geometry instances.
- The intersection set is interior to both source geometry instances.

#### Geometry Examples

This example uses `STCrosses()` to test two geometry instances to see if they cross:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0)', 0);
SET @h = geometry::STGeomFromText('LINESTRING(0 0, 2 2)', 0);
SELECT @g.STCrosses(@h);
```

## STCurveToLine (Geometry)&#x20;

`STCurveToLine()`returns a polygonal approximation of a geometry instance that contains circular arc segments.

#### Syntax

```sql
.STCurveToLine ( )
```

#### Return Types

CQL: geometry

#### Remarks

Returns null for uninitialized geometry variables&#x20;

The polygonal approximation that the method returns depends on the geometry instance used to call the method:&#x20;

- Returns a `LineString` instance for a `CircularString`or `CompoundCurve`instance.
- Returns a `Polygon`instance for a `CurvePolygon`instance.
- Returns a copy of the geometry instance if that instance is not a `CircularString`, `CompoundCurve`, or `CurvePolygon`instance.&#x20;

Any z-coordinate values present in the calling geometry instance are ignored.

#### Geometry Example

In this example, the SELECT statement uses a `LineString` instance to call the `STCurveToLine`method. Thus, the method returns a `LineString`instance:

```sql
 DECLARE @g geometry;
 SET @g = geometry::Parse('LINESTRING(1 3, 5 5, 4 3, 1 3)');
 SET @g = @g.STCurveToLine();
 SELECT @g.STGeometryType();
```

## STDifference (Geometry)&#x20;

`STDifference()` returns an object that represents the point set from one geometry instance that does not lie within another geometry instance.

#### Syntax

```sql
.STDifference ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STDifference()` is invoked.

#### Return Types

CQL: geometry

#### Remarks

Returns null if the spatial reference IDs (SRIDs) of the geometry instances do not match.&#x20;

#### Geometry Example

This example uses `STDifference()` to compute the difference between two `Polygons`:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 0 2, 2 2, 2 0, 0 0))', 0);
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);
SELECT @g.STDifference(@h).ToString();
```

## STDisjoint (Geometry)&#x20;

`STDisjoint()`returns 1 if a geometry instance is spatially disjoint from another geometry instance, returns 0 if it is not.

#### Syntax

```sql
.STDisjoint ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STDisjoint()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

If the intersection of the two geometry instances point sets are empty, they are disjoint.&#x20;

Returns null if the spatial reference IDs (SRIDs) of the geometry instances do not match.

#### Geometry Example

This example uses `STDisjoint()` to test two geometry instances for spatial disjoint:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);
SET @h = geometry::STGeomFromText('POINT(1 1)', 0);
SELECT @g.STDisjoint(@h);
```

## STDistance

`STDistance()` returns the shortest distance between a point in a geometry/geography instance and a point in another geometry/geography instance.

#### Syntax

```sql
.STDistance ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry/geography instance to compare against the instance on which `STDistance()` is invoked.

#### Return Types

CQL: Number

#### Remarks

`STDistance()` always returns null if the spatial reference IDs (SRIDs) of the geometry/geography instances do not match.

#### Geometry Example

This example finds the distance between two geometry instances:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 2 0, 2 2, 0 2, 0 0))', 0);
SET @h = geometry::STGeomFromText('POINT(10 10)', 0);
SELECT @g.STDistance(@h);
```

#### Geography Example

&#x20;This example finds the distance between two geography instances:

```sql
DECLARE @g geography;
DECLARE @h geography;
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);
SET @h = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);
SELECT @g.STDistance(@h);
```

## STEndpoint (Geometry)&#x20;

`STEndPoint()`returns the end point of a geometry instance.

#### Syntax

```sql
.STEndPoint ( )
```

#### Return Types

CQL: geometry

#### Remarks

`STEndPoint()` is the equivalent of [`STPointN()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STPointN).&#x20;

Returns null if called on an empty geometry instance.&#x20;

#### Geometry Example

This example creates a `LineString` instance with [`STGeomFromText()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STGeomFromText) and uses `STEndpoint()` to retrieve the end point of the `LineString:`

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);
SELECT @g.STEndPoint().ToString();
```

## STEnvelope (Geometry)

`STEnvelope()`returns the minimum axis-aligned bounding rectangle of the instance.

#### Syntax

```sql
STEnvelope ( )
```

#### Return Types

CQL: geometry&#x20;

#### Geometry Example

This example uses [`STGeomFromText()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STGeomFromText) to create a `LineString` instance from (0,0) to (2,3), and uses `STEnvelope()` to return the bounding box of the `LineString:`

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 3)', 0);
SELECT @g.STEnvelope().ToString();
```

## STEquals (Geometry)&#x20;

`STEquals()` returns 1 if a geometry instance represents the same point set as another geometry instance, returns 0 if it does not.

#### Syntax

```sql
.STEquals ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STEquals()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

Returns null if the spatial reference IDs (SRIDs) of the geometry instances do not match.

#### Geometry Example

This example creates two geometry instances with [`STGeomFromText()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STGeomFromText) that are equal but not trivially equal, and uses `STEquals()` to test their equality:

```sql
DECLARE @g geometry
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);
SET @h = geometry::STGeomFromText('MULTILINESTRING((4 2, 2 0), (0 2, 2 0))', 0);
SELECT @g.STEquals(@h);
```

## STExteriorRing (Geometry)

`STExteriorRing()`returns the exterior ring of a geometry instance that is a `Polygon`.

#### Syntax&#x20;

```sql
.STExteriorRing ( )
```

#### Return Types

CQL: geometry &#x20;

#### Remarks

Returns null if the geometry instance is not a `Polygon`.

#### Geometry Example

This example creates a polygon instance and uses `STExteriorRing()` to return the exterior ring of the polygon as a `LineString`:&#x20;

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);
SELECT @g.STExteriorRing().ToString();
```

## STGeometryN (Geometry)&#x20;

`STGeometryN()` returns a specified geometry in a geometry collection.

#### Syntax

```sql
.STGeometryN ( expression )
```

#### Arguments

_expression_\
Is an int expression between 1 and the number of geometry instances in the `GeometryCollection`.

#### Return Types

CQL: geometry

#### Remarks

Returns null if the parameter is larger than the result of `STGeometryN()` and will throw an `ArgumentOutOfRangeException`if the _expression_ parameter is less than 1.

#### Geometry Example

This example creates a `MultiPoint` `GeometryCollection`and uses `STGeometryN()` to find the second geometry instance of the collection:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('MULTIPOINT(0 0, 13.5 2, 7 19)', 0);
SELECT @g.STGeometryN(2).ToString();
```

## STGeometryType (Geometry)&#x20;

`STGeometryType()`returns the Open Geospatial Consortium (OGC) type name represented by geometry instance.

#### Syntax&#x20;

```sql
.STGeometryType ( )
```

#### Return Types

CQL: Text

#### Remarks

The OGC type names that can be returned by `STGeometryType()` are `Point`, `LineString`, `CircularString`, `CompoundCurve`, `Polygon`, `CurvePolygon`, `GeometryCollection`, `MultiPoint`, `MultiLineString`, `MultiPolygon`, and `FullGlobe`.

#### Geometry Example

This example creates a `Polygon` instance and uses `STGeometryType()` to confirm that it is a `Polygon:`

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0))', 0);
SELECT @g.STGeometryType();
```

## STGeomCollFromText (Geometry)

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance, `STGeomCollFromText()`returns a geometry instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax&#x20;

```sql
STGeomCollFromText ( 'geometrycollection_tagged_text' , SRID )
```

#### Arguments

_geometrycollection_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry instance you wish to return.

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry instance you wish to return.

#### Return Types

CQL: geometry

#### Remarks

The OGC type of the geometry instance returned by `STGeomCollFromText()` is set to the corresponding WKT input.

Throws an `ArgumentException` if the input is not valid.

#### Geometry Example

This example uses `STGeomCollFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomCollFromText('GEOMETRYCOLLECTION ( POLYGON((5 5, 10 5, 10 10, 5 5)), POINT(10 10) )', 0);
SELECT @g.ToString();
```

## STGeomFromText

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance, `STGeomFromText()`returns a geometry/geography instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax&#x20;

```sql
STGeomFromText ( 'instance_tagged_text' , SRID )
```

#### Arguments

_inctance_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry/geography instance you wish to return.

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry/geography instance you wish to return.

#### Return Types

CQL: geometry/geography

#### Remarks

The OGC type of the geometry/geography instance returned by `STGeomFromText()` is set to the corresponding WKT input.

If the input isn't well-formatted, method will throw a `FormatException`.

Geography:&#x20;

Throws an `ArgumentException` if the input contains an antipodal edge.

#### Geometry Example

This example uses `STGeomFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING (100 100, 20 180, 180 180)', 0);
SELECT @g.ToString();
```

#### Geography Example

This example uses `STGeomCollFromText()` to create a geography instance:

```sql
DECLARE @g geography;
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);
SELECT @g.ToString();
```

## STGeomFromWKB

`STGeomFromWKB()`returns a geometry/geography instance from an Open Geospatial Consortium (OGC) Well-Known Binary (WKB) representation.

#### Syntax&#x20;

```sql
STGeomFromWKB ( 'WKB_instance' , SRID )
```

#### Arguments&#x20;

_WKB_instance_\
An nvarchar(max) expression that is the WKB representation of the geometry/geography instance to return.

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry/geography instance to return.

#### Return Types

CQL: geometry/geography

#### Remarks

The OGC type of the geometry/geography instance returned by `STGeomFromWKB()` is set to the corresponding WKB input.

If the input isn't well-formatted, method will throw a `FormatException`.

Geography:&#x20;

Throws an `ArgumentException` if the input contains an antipodal edge.

#### Geometry Example

This example uses `STGeomFromWKB()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromWKB(0x010200000003000000000000000000594000000000000059400000000000003440000000000080664000000000008066400000000000806640, 0);
SELECT @g.STAsText();
```

#### Geography Example

This example uses `STGeomFromWKB()` to create a geography instance:

```sql
DECLARE @g geography;
SET @g = geography::STGeomFromWKB(0x010200000002000000D7A3703D0A975EC08716D9CEF7D34740CBA145B6F3955EC08716D9CEF7D34740, 4326);
SELECT @g.ToString();
```

## STInteriorRingN (Geometry)

`STInteriorRingN()` returns the specified interior ring of a `Polygon`geometry instance.

#### Syntax&#x20;

```sql
.STInteriorRingN ( expression )
```

#### Arguments&#x20;

_expression_\
An int expression between 1 and the number of interior rings in the geometry instance.

#### Return Types

CQL: geometry&#x20;

#### Remarks

Returns null if the geometry instance is not a `Polygon`.&#x20;

This method will throw an `ArgumentOutOfRangeException`if the expression is larger than the number of rings. The number of rings can be returned using [`STNumInteriorRing()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STNumInteriorRing).

#### Geometry Example

This example creates a `Polygon` instance and uses `STInteriorRingN()` to return the interior ring of the `Polygon`as a `LineString`:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);
SELECT @g.STInteriorRingN(1).ToString();
```

## STIntersection

`STIntersection()` returns an object that represents the points where a geometry/geography instance intersects another geometry/geography instance.

#### Syntax&#x20;

```sql
.STIntersection ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry/geography instance to compare against the instance on which `STIntersection()` is invoked.

#### Return Types

CQL: geometry/geography

#### Remarks

If the spatial reference IDs (SRIDs) of the geometry/geography instances do not match, `STIntersection()` always returns null.&#x20;

The result may contain circular arc segments only if the input instances contain them.

#### Geometry Example

&#x20;This example uses `STIntersection()` to compute the intersection of two polygons:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 0 2, 2 2, 2 0, 0 0))', 0);
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);
SELECT @g.STIntersection(@h).ToString();
```

#### Geography Example

This example uses `STIntersection()` to compute the intersection of a `Polygon` and a `LineString:`

```sql
DECLARE @g geography;
DECLARE @h geography;
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);
SET @h = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);
SELECT @g.STIntersection(@h).ToString();
```

## STIntersects

`STIntersects()` returns 1 if a geometry instance intersects another geometry instance. Returns 0 if it does not.

#### Syntax

```sql
.STIntersects ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry/geography instance to compare against the instance on which `STIntersects()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

Returns null if the spatial reference IDs (SRIDs) of the geometry/geography instances do not match.

#### Geometry Example

This example uses `STIntersects()` to determine if two geometry instances intersect each other:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);
SET @h = geometry::STGeomFromText('POINT(1 1)', 0);
SELECT @g.STIntersects(@h);
```

#### Geography Example&#x20;

This example uses `STIntersects()` to determine whether two geography instances intersect each other:

```sql
DECLARE @g geography;
DECLARE @h geography;
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);
SET @h = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);

SELECT CASE @g.STIntersects(@h)
WHEN 1 THEN '@g intersects @h'
ELSE '@g does not intersect @h'
END;
```

## STIsClosed (Geometry)&#x20;

`STIsClosed()`returns 1 if the start and end points of the given geometry instance are the same. Returns 1 for `GeometryCollection` types if each contained geometry instance is closed. Returns 0 if the instance is not closed.

#### Syntax

```sql
.STIsClosed ( )
```

#### Return Types

CQL: Yes/No

#### Remarks

Returns 0 if any figures of a geometry instance are points, or if the instance is empty.

All `Polygon`instances are considered closed.

#### Geometry Example&#x20;

This example creates a `LineString` instance and uses `STIsClosed()` to test if the `LineString` is closed:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);
SELECT @g.STIsClosed();
```

## STIsEmpty (Geometry)&#x20;

`STIsEmpty()`returns 1 if a geometry instance is empty. Returns 0 if a geometry instance is not empty.

#### Syntax

```sql
.STIsEmpty ( )
```

#### Return Types

CQL: Yes/No

#### Geometry Example

This example creates an empty geometry instance and uses `STIsEmpty()` to test whether the instance is empty:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON EMPTY', 0);
SELECT @g.STIsEmpty();
```

## STIsRing (Geometry)

`STIsRing()` returns 1 if a geometry instance fulfills the following requirements:

- It is a `LineString`instance.
- It is closed (for a geometry to be closed, STIsClosed() needs to return 1 when invoked on the instance).
- It is simple (for a geometry to be simple, STIsSimple() needs to return 1 when invoked on the instance).
- Returns 0 if the `LineString`instance does not meet the requirements.

#### Syntax&#x20;

```sql
.STIsRing ( )
```

#### Return Types

CQL: Yes/No

#### Remarks

&#x20;Returns null if the instance is not a `LineString`.

#### Geometry Example

This example creates a `LineString` instance and uses `STIsRing()` to test whether the instance is a ring:&#x20;

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0, 0 0)', 0);
SELECT @g.STIsRing();
```

## STIsSimple (Geometry)

`STIsSimple()` returns 1 if a geometry instance is simple, as defined by the Open Geospatial Consortium (OGC). Returns 0 if a geometry instance is not simple.

#### Syntax

```sql
.STIsSimple ( )
```

#### Return Types

CQL: Yes/No

#### Remarks

To be simple a geometry instance must meet the requirements:

- Except at the endpoints, each figure of the instance must not intersect itself.
- No two figures of the instance can intersect each other at a point that is not in both of their boundaries.

#### Geometry Example

This example creates a non-simple `LineString` instance that intersects itself and uses `STIsSimple()` to test whether the `LineString` is simple:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 0 2, 2 0)', 0);
SELECT @g.STIsSimple();
```

## STIsValid (Geometry)&#x20;

`STIsValid()`returns true if a geometry instance is well-formed, based on its Open Geospatial Consortium (OGC) type. Returns false if a geometry instance is not well-formed.

#### Syntax

```sql
.STIsValid ( )
```

#### Return Types

CQL: Yes/No

#### Remarks

The OGC type of a geometry instance can be determined by invoking [`STGeometryType()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STGeometryType)`.`

SQL Server produces only valid geometry instances, but allows for the storage and retrieval of invalid instances.

#### Geometry Example

This example creates a geometry instance and uses `STIsValid()` to test if the instance is valid:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);
SELECT @g.STIsValid();
```

## STLength

`STLength()`returns the total length of the elements in a geometry/geography instance or the geometry/geography instances within a `GeometryCollection`.

#### Syntax

```sql
.STLength ( )
```

#### Return Types

CQL: Yes/No

#### Remarks

If a geometry/geography instance is closed, its length is calculated as the total length around the instance

The length of a `GeometryCollection`is found by calculating the sum of the lengths of all of the geometry/geography instances contained within the collection.

`STLength()` works on both valid and invalid `LineString`.

#### Geometry Example

This example creates a `LineString` instance and uses `STLength()` to find the length of the instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);
SELECT @g.STLength();
```

#### Geography Example

This example creates a `LineString` instance and uses `STLength()` to find the length of the instance:

```sql
DECLARE @g geography;
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);
SELECT @g.STLength();
```

## STLineFromText (Geometry)

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance,`STLineFromText()`returns a geometry instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax

```sql
STLineFromText ( 'linestring_tagged_text' , SRID )
```

#### Arguments&#x20;

_linestring_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry `LineString`instance you wish to return.&#x20;

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry `LineString` instance you want to return.

#### Return Types

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Geometry Example

This example uses `STLineFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STLineFromText('LINESTRING (100 100, 200 200)', 0);
SELECT @g.ToString();
```

## STLineFromWKB (Geometry)

`STLineFromWKB()`returns a geometry `LineString`instance from an Open Geospatial Consortium (OGC) Well-Known Binary (WKB) representation.

#### Syntax

```sql
STLineFromWKB ( 'WKB_linestring' , SRID )
```

#### Arguments&#x20;

_WKB_linestring_\
A varbinary(max) expression that is the WKB representation of the geometry `LineString`instance to return.

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry `LineString`instance you want to return.

#### Return Types

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Geometry Example

This example uses `STLineFromWKB()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STLineFromWKB(0x0102000000020000000000000000005940000000000000594000000000000069400000000000006940, 0);
SELECT @g.STAsText();
```

## STMLineFromText (Geometry)

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance, `STMLineFromText()` returns a geometry instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax

```sql
STMLineFromText ( 'multilinestring_tagged_text' , SRID )
```

#### Arguments&#x20;

_multilinestring_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry`MultiLineString`instance you wish to return.&#x20;

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry `MultiLineString` instance you wish to return.

#### Return Types

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Geometry Example&#x20;

This example uses `STMLineFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STMLineFromText('MULTILINESTRING ((100 100, 200 200), (3 4, 7 8, 10 10))', 0);
SELECT @g.ToString();
```

## STMPointFromText (Geometry)

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance, `STMPointFromText()`returns a geometry instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax&#x20;

```sql
STMPointFromText ( 'multipoint_tagged_text', SRID )
```

#### Arguments&#x20;

_multipoint_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry `MultiPoint` instance you wish to return.&#x20;

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry`MultiPoint` instance you wish to return.

#### Return Types

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Geometry Example

This example uses `STMPointFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STMPointFromText('MULTIPOINT ((100 100), (200 200))', 0);
SELECT @g.ToString();
```

## STMPolyFromText (Geometry)

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance, `STMPolyFromText()`returns a geometry instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax

```sql
STMPolyFromText ( 'multipolygon_tagged_text' , SRID )
```

#### Arguments

_multipolygon_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry `MultiPolygon` instance you wish to return.&#x20;

_SRID_\
Is an int expression representing the spatial reference ID (SRID) of the geometry `MultiPolygon` instance you wish to return.

#### ReturnTypes

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Example

This example uses`STMPolyFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STMPolyFromText('MULTIPOLYGON (((5 5, 10 5, 10 10, 5 5)), ((10 10, 100 10, 200 200, 30 30, 10 10)))', 0);
SELECT @g.ToString();
```

## STNumCurves (Geometry)&#x20;

`STNumCurves()` returns the number of curves in a one-dimensional geometry instance.

#### Syntax

```sql
.STNumCurves()
```

#### Return Types

CQL: geometry

#### Remarks

An empty one-dimensional geometry instance returns 0.

Null is returned when the geometry instance is not a one-dimensional instance or is an uninitialized instance.

One-dimensional spatial data types include `LineString`, `CircularString`, and `CompoundCurve`. `STNumCurves()` works only on simple types; it does not work with geometry collections like `MultiLineString`.

#### Example

This example shows how to get the number of curves in a `CircularString` instance:

```sql
 DECLARE @g geometry;
 SET @g = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)');
 SELECT @g.STNumCurves();
```

## STNumGeometries (Geometry)&#x20;

`STNumGeometries()` returns the number of geometries that comprise a geometry instance.

#### Syntax

```sql
.STNumGeometries ( )
```

#### Return Types

CQL: Number

#### Remarks

This method returns 1 if the geometry instance is not a `MultiPoint`, `MultiLineString`, `MultiPolygon`, or `GeometryCollection`instance, and 0 if the geometry instance is empty.

#### Example&#x20;

This example creates a `MultiPoint` instance and uses `STNumGeometries()` to find out how many geometries the instance contains:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('MULTIPOINT((-122.360 47.656), (-122.343 47.656))', 4326);
SELECT @g.STNumGeometries();
```

## STNumInteriorRing (Geometry)

`STNumInteriorRing()` returns the number of interior rings of a `Polygon`geometry instance.

#### Syntax

```sql
.STNumInteriorRing ( )
```

#### Return Types

CQL: Number

#### Remarks

&#x20;Returns null if the geometry instance is not a `Polygon`.

#### Example

This example creates a `Polygon` instance and uses `STNumInteriorRing()` to find how many interior rings the instance has:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);
SELECT @g.STNumInteriorRing();
```

## STNumPoints (Geometry)&#x20;

`STNumPoints()` returns the sum of the number of points in each of the figures in a geometry instance.

#### Syntax

```sql
.STNumPoints ( )
```

#### Return Types

CQL: Number

#### Remarks

`STNumPoints()` counts the points (duplicate points are counted) in the description of a geometry instance. If this instance is a collection type, this method returns the sum of the points in each of its elements.

#### Example

This example creates a `LineString` instance and uses `STNumPoints()` to determine how many points were used in the description of the instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);
SELECT @g.STNumPoints();
```

## STOverlaps (Geometry)

`STOveralps()`returns 1 if a geometry instance overlaps another geometry instance. Returns 0 if it does not.

#### Syntax

```sql
.STOverlaps ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STOverlaps()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

If the points where the geometry instances intersect are not in the same dimension, `STOverlaps()` always returns 0.

If the spatial reference IDs (SRIDs) of the geometry instances do not match, `STOverlaps()` returns null.

Two geometry instances overlap if the region representing their intersection has the same dimension as the instances do and the region does not equal either instance.

#### Example

This example uses `STOverlaps()` to test two geometry instances for overlap:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 2 0, 2 2, 0 2, 0 0))', 0);
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);
SELECT @g.STOverlaps(@h);
```

## **STPointFromText (Geometry)**&#x20;

{% hint style="warning" %}
Only available in SQL Server implementations.
{% endhint %}

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance, `STPointFromText()` returns a geometry instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax

```sql
STPointFromText ( 'point_tagged_text' , SRID )
```

#### Arguments

_point_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry `Point` instance you wish to return.&#x20;

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry `Point`instance you wish to return.

#### ReturnTypes

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Example

This example uses`STPointFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STPointFromText('POINT (100 100)', 0);
SELECT @g.ToString();
```

## STPointFromWKB (Geometry)&#x20;

`STPointFromWKB()`returns a geometry Point instance from an Open Geospatial Consortium (OGC) Well-Known Binary (WKB) representation.

#### Syntax

```sql
STPointFromWKB ( 'WKB_point' , SRID )
```

#### Arguments

_WKB_point_\
A varbinary(max) expression that is the WKB representation of the geometry `Point` instance you wish to return.

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry `Point` instance you wish to return.

#### Return Types

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Example

This example uses`STPointFromWKB()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STPointFromWKB(0x010100000000000000000059400000000000005940, 0);
SELECT @g.STAsText();
```

## STPointN (Geometry)&#x20;

`STPointN()`returns a specified point in a geometry instance.

#### Syntax

```sql
.STPointN ( expression )
```

#### Arguments&#x20;

_expression_\
An int expression between 1 and the number of points in the geometry instance.

#### Return Types

CQL: geometry

#### Remarks

Throws an `ArgumentOutOfRangeException`, if this method is called with a value less than 1.

Returns null if this method is called with a value greater than the number of points in the instance.

`STPointN()` returns the point specified by expression, if a geometry instance is user created. (occurs by ordering the points in which they were originally input).

`STPointN()` returns the point specified by _expression,_ if a geometry instance was constructed by the system (by ordering all the points in the same order they would be output: first by geometry, then by ring within the instance (if appropriate), and then by point within the ring).

#### Example

This example creates a `LineString` instance and uses `STPointN()` to retrieve the second point in the description of the instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);
SELECT @g.STPointN(2).ToString();
```

## STPointOnSurface (Geometry)

`STPointOnSurface()`returns an arbitrary point located within the interior of a geometry instance.

#### Syntax

```sql
.STPointOnSurface ( )
```

#### Return Types

CQL: geometry

#### Remarks

If the instance is empty, method returns null.

#### Example

This example creates a `Polygon` instance and uses `STPointOnSurface()` to find a point on the instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);
SELECT @g.STPointOnSurface().ToString();
```

## STPolyFromText (Geometry)

{% hint style="warning" %}
Only available in SQL Server implementations.
{% endhint %}

Augmented with any `Z` (elevation) and `M` (measure) values carried by the instance, `STPolyFromText()`returns a geometry instance from an Open Geospatial Consortium (OGC) Well-Known Text (WKT) representation.

#### Syntax

```sql
STPolyFromText ( 'polygon_tagged_text' , SRID )
```

#### Arguments

_polygon_tagged_text_\
An nvarchar(max) expression that is the WKT representation of the geometry `Polygon`instance you wish to return.

_SRID_\
An int expression representing the spatial reference ID (SRID) of the geometry `Polygon` instance you wish to return.

#### Return Types

CQL: geometry

#### Remarks

If the input isn't well-formatted, method will throw a `FormatException`.

#### Example

This example uses`STPolyFromText()` to create a geometry instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STPolyFromText('POLYGON ((5 5, 10 5, 10 10, 5 5))', 0);
SELECT @g.ToString();
```

## STRelate (Geometry)

`STRelate()`returns 1 if a geometry instance is related to another geometry instance, otherwise, returns 0. (The relationship between the geometry instances is defined by a Dimensionally Extended 9 Intersection Model (DE-9IM) pattern matrix value)

#### Syntax

```sql
.STRelate ( other_instance, intersection_pattern_matrix )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STRelate()` is invoked.

_intersection_pattern_matrix_\
Is a string of type nchar(9) encoding acceptable values for the DE-9IM pattern matrix device between the two geometry instances.

#### Return Types

CQL: Yes/No

#### Remarks

If the spatial reference IDs (SRIDs) of the geometry instances do not match, method returns null.

If matrix is not well-formed, an `ArgumentException`will be thrown.

#### Example

This example uses `STRelate()` to test two geometry instances for spatial disjoint using an explicit DE-9IM pattern:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);
SET @h = geometry::STGeomFromText('POINT(5 5)', 0);
SELECT @g.STRelate(@h, 'FF*FF****');
```

## STStartPoint (Geometry)&#x20;

`STStartPoint()`returns the start point of a geometry instance.

#### Syntax

```sql
.STStartPoint ( )
```

#### Return Types

CQL: geometry

#### Remarks

`STStartPoint()` is the equivalent of [`STPointN()`](https://app.gitbook.com/@cinchy/s/cql/~/drafts/-MT7EiwgHRMJYvTITUSx/functions/geometry-and-geography-data-type-and-functions/ogc-methods-on-geometry-and-geography-instances#STPointN).

#### Example

This example uses `STStartPoint()` to retrieve the start point of the instance and creates a `LineString` instance:

```sql
DECLARE @g geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);
SELECT @g.STStartPoint().ToString();
```

## STSymDifference (Geometry)&#x20;

`STSymDifference()`returns an object that represents all points that are either in one geometry instance or another geometry instance, but not those points that lie in both instances.

#### Syntax

```sql
.STSymDifference ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STSymDifference()` is invoked.

#### Return Types

CQL: geometry/geography

#### Remarks

If the spatial reference IDs (SRIDs) of the geometry instances do not match, method returns null.&#x20;

Result may contain circular arc segments (only if the input instances contain circular arc segments).

#### Example

This example uses `STSymDifference()` to compute the symmetric difference of two `Polygon` instances:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 0 2, 2 2, 2 0, 0 0))', 0);
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);
SELECT @g.STSymDifference(@h).ToString();
```

## STTouches (Geometry)

`STTouches()`returns 1 if a geometry instance spatially touches another geometry instance. Returns 0 if it does not.

#### Syntax

```sql
.STTouches ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STTouches()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

If two geometry instances point sets intersect, they are touching but their interiors do not intersect.

If the spatial reference IDs (SRIDs) of the geometry instances do not match, method returns null.&#x20;

#### Example

This example uses `STTouches()` to test two `geometry` instances to see if they touch:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);
SET @h = geometry::STGeomFromText('POINT(1 1)', 0);
SELECT @g.STTouches(@h);
```

## STUnion (Geometry)&#x20;

`STUnion()`returns an object that represents the union of a geometry instance with another geometry instance.

#### Syntax

```sql
.STUnion ( other_instance )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STUnion()` is invoked.

#### Return Types

CQL: geometry

#### Remarks

If the spatial reference IDs (SRIDs) of the geometry instances do not match, method returns null.&#x20;

Result may contain circular arc segments (only if the input instances contain circular arc segments).

#### Example

This example uses `STUnion()` to compute the union of two `Polygon` instances:&#x20;

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 0 2, 2 2, 2 0, 0 0))', 0);
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);
SELECT @g.STUnion(@h).ToString();
```

## STWithin (Geometry)

`STWithin()`returns 1 if a geometry instance is completely within another geometry instance; otherwise, returns 0.&#x20;

#### Syntax

```sql
STWithin ( other_instances )
```

#### Arguments

_other_instance_\
Another geometry instance to compare against the instance on which `STWithin()` is invoked.

#### Return Types

CQL: Yes/No

#### Remarks

The `STWithin` command is case-sensitive.

If the spatial reference IDs (SRIDs) of the geometry instances do not match, method returns null.

#### Example

This example uses `STWithin()` to test two `geometry` instances to see if the first instance is completely within the second instance:

```sql
DECLARE @g geometry;
DECLARE @h geometry;
SET @g = geometry::STGeomFromText('POLYGON((0 0, 2 0, 2 2, 0 2, 0 0))', 0);
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);
SELECT @g.STWithin(@h);
```
