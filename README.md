# MetOcean API conventions, version 1.0.0
# Versioning
All APIs must use a version base path. This allows for graceful upgrades later on.
```
E.g.
http://myserver:1111/v0/object/{object_id}
http://myserver:1111/v1/object/{object_id}
```
All API design and configuration must allow for parallel deployment of more than one version.

# Verbs
POST creates new, rejects edit
PUT creates new or replaces
PATCH edits

# HTTP status / response codes:

https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html

# Query components
This follows http://www.opensearch.org/Specifications/OpenSearch/Extensions/Geo/1.0/Draft_2 with some extensions. Ordering is consistent with WMS. Longitudes must be in the interval -180.<=lon<=180.
All intervals include the endpoints.
## Time selection
### Time range
Select by time range
```
time={ISO8601 time interval with double dash option and empty start/end allowed}
E.g.
time=2007-03-01T13:00:00Z--2008-05-11T15:30:00Z
time=2007-03-01T13:00:00Z--
time=--2008-05-11T15:30:00Z
```

## Time interpolation
### Times
Interpolate for a series of discrete times
```
times=[ISO8601 times]
E.g:
times=2007-03-01T13:00:00Z,2008-05-11T15:30:00Z
```

### Time series
Interpolate for a regularly spaced time series
Time range with interval
```
times={ISO8601 time range with intervals}
E.g.:
times=R13--2014-01-01T00:00:00--PT3H  (means get 13 timesteps of 3 Hour interval starting at 2014-01-01 00:00:00)
```

## Geographical selection
### Bounding box
Select by bounding box.
```
bbox={W,S,E,N as decimal longitudes and latitudes}
E.g.:
bbox=165.0,-40.0,180.0,-35.0
```

### Point and radius
Select by point and radius
```
lon={decimal longitude}&lat={decimal latitude}&radius={radius in meters}
E.g.:
lon=165.5&lat=-34.5&radius=10000
```

## Variable selection
### Select by variable
Use CF convention ids (no capitalisations)
```
vars=[cf_variable_ids]
E.g.
vars=sea_surface_wave_significant_height,eastward_sea_water_velocity_assuming_no_tide
```
## Cycle selection
### Select by specific cycle
```
cycle={ISO8601 cycle time} (if omitted should default to latest)
E.g.
cycle=20170201T000000Z
```

### Select by previous cycle
```
cycle_past={index of preceding cycle} (starting with 0 index on latest and counting backwards)
E.g.
cycle_past=1 (for cycle before latest)
```


## Geographical extraction/interpolation
### Series of points
Interpolate onto series of points
```
lons=[decimal longitudes]&lats=[decimal latitudes]
E.g.:
lons=165.5,170.2,180.4&lats=-34.5,-36.0,-40.2
```

### Trajectory
Interpolate onto a trajectory
```
lons=[decimal longitudes]&lats=[decimal latitudes]&times=[ISO8601 times]
E.g.:
lons=165.5,170.2,180.4&lats=-34.5,-36.0,-40.2&times=2013-01-05T00:00:00Z,2013-01-05T00:06:00Z,2013-01-05T00:12:00Z
```

### Grid
Interpolate onto a regular grid. Grid is constructed from SW corner to include but not extend past the NE corner
```
bbox={W,S,E,N as decimal longitude and latitudes}&dlon={decimal longitude spacing}&dlat={decimal longitude spacing}
E.g.:
bbox=165.0,-40.0,180.0,-35.0&dlon=1.0&dlat=1.0
```

### Level interpolation
Interpolate onto a level
```
lev={level in dataset units}
E.g.:
lev=100
```

# MIME types
APIs must include correct MIME types in their responses and these correspond to the internal definitions used by the MDS.
Accepted types include [all standard media types](http://www.iana.org/assignments/media-types/media-types.xhtml) and the following extensions:

* application/vnd.wmo.grib+binary
* application/vnd.unidata.netcdf+binary
* application/vnd.wmo.grib2+binary
* application/vnd.metocean.cf+json
* text/vnd.metocean.spectra.swan+plain
* text/vnd.metocean.spectra.octopus+plain

These can be added and or subtyped for specific formats

Note CF-JSON is just a specification within JSON, and is specified as above to differentiate it from any other arbitrary JSON which would be application/json.

Optionally, a cf suffix can be added to netcdf to indicate that it is cf compliant:
* application/vnd.unidata.netcdf.cf+binary

Where an endpoint supports multiple MIME types, the requested type should be specified in the 'Content-Type' header of the request.

# Pagination
Pagination should be implemented when it's necessary to limit the number of data points returned on a single request, usually this is necessary when the amount of data being returned/processed, exceeds the acceptable time/resources limits of a given API implementation. 

## Query string: limit & offset

The current proposed solution is to introduce 2 query paramaters to control the size and iteration of each "page": `limit` and `offset`, where the `limit` is the maximum number of records is expected to be returned and `offset` the starting point where to count `limit` from. 

For example, let's suppose that there's a total of 1000 data points returned from a certain query, but we user want to fetch only every 100 points, the first query would be:

```
?limit=100&offset=0&...
```
Would return the first 100 records. Then the second "page" query:

```
?limit=100&offset=100&...
```
Returning data-points from 100-200

This assumes that there's an default order of data-points, which in our case is usually `time` for metocean data, but could be that the query is orderd by ID, which could be a number or a string.

## Header Link helpers

The initial suggestion is that is provided helper Links in the headers to facilitate iteration through the pages following the :

```
Link: <{protocol}://{host}/{endpoint}/?...&limit=100&offset=200>; rel="next", <{protocol}://{host}/{endpoint}/?...&limit=100&offset=0>; rel="prev"
```
Where "prev" (previous) link can (or should?) be omitted in the first page and "next" omitted in the second page.
