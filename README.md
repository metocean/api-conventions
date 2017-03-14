# api-conventions
# Versioning
All APIs must use a version base path. This allows for graceful upgrades later on.
```
E.g.
http://myserver:1111/v0/object/{object_id}
http://myserver:1111/v1/object/{object_id}
```
All API design and configuration must allow for parallel deployment of more than one version.

# Query components
This follows http://www.opensearch.org/Specifications/OpenSearch/Extensions/Geo/1.0/Draft_2 with some extensions. Ordering is consistent with WMS. Longitudes must be in the interval -180.<=lon<=180.
All intervals include the endpoints.
##Time selection
###Time range
Select by time range
```
time={ISO8601 time range with double dash option and empty start/end allowed}
E.g.
time=2007-03-01T13:00:00Z--2008-05-11T15:30:00Z
time=2007-03-01T13:00:00Z--
time=--2008-05-11T15:30:00Z
```

##Time interpolation
###Time
Interpolate for single time
```
time={ISO8601 time}
E.g: 
time=2007-03-01T13:00:00Z
```

###Times
Interpolate for a series of discrete times
```
times=[ISO8601 times]
E.g:
times=2007-03-01T13:00:00Z,2008-05-11T15:30:00Z
```

###Time series
Interpolate for a regularly spaced time series
Time range with interval
```
time={ISO8601 time range}&interval={ISO8601 duration}
E.g.:
time=2007-03-01T13:00:00Z/2008-05-11T15:30:00Z&interval=PT3H
```

##Geographical selection
###Bounding box
Select by bounding box.
```
bbox={W,S,E,N as decimal longitude and latitudes}
E.g.:
bbox=165.0,-40.0,180.0,-35.0
```

###Point and radius
Select by point and radius
```
lon={decimal longitude}&lat={decimal latitiude}&radius={radius in meters}
E.g.:
lon=165.5&lat=-34.5&radius=10000
```

##Variable selection
###Select by variable
Use CF convention ids (no capitalisations)
```
vars=[cf_variable_ids]
E.g.
vars=sea_surface_wave_significant_height,eastward_sea_water_velocity_assuming_no_tide
```
##Cycle selection
###Select by specific cycle
```
cycle={ISO8601 cycle time} (If omitted should default to latest)
E.g.
cycle=20170201T000000Z
```

###Select by previous cycle
```
cycle_past={index of preceding cycle} (Starting with 0 index on latest and counting backwards)
E.g.
cycle_past=1 (For cycle before latest)
```


##Geographical extraction/interpolation
###Series of points
Interpolate onto series of points
```
lons=[decimal longitudes]&lats=[decimal latitiude]
E.g.:
lons=165.5,170.2,180.4&lats=-34.5,-36.0,-40.2
```

###Trajectory
Interpolate onto a trajectory
```
lons=[decimal longitudes]&lats=[decimal latitudes]&times=[ISO8601 times]
E.g.:
lons=165.5,170.2,180.4&lats=-34.5,-36.0,-40.2&times=2013-01-05T00:00:00Z,2013-01-05T00:06:00Z,2013-01-05T00:12:00Z
```

###Grid
Interpolate onto a regular grid. Grid is constructed from SW corner to include but not extend past the NE corner
```
bbox={W,S,E,N as decimal longitude and latitudes}&dlon={decimal longitude spacing}&dlat={decimal longitude spacing}
E.g.:
bbox=165.0,-40.0,180.0,-35.0&dlon=1.0&dlat=1.0
```

###Level interpolation
Interpolate onto a level
```
lev={level in dataset units}
E.g.:
lev=100
```

#Mime types
APIs must include correct MIME tyeps in their responses and these correspond to the internal definitions used by the MDS.
Accepted types include [all standard media types](http://www.iana.org/assignments/media-types/media-types.xhtml) and the following extensions:

* application/x-netcdf
* application/x-netcdf4
* application/x-grib
* application/x-grib2
* text/x-swan-spectra

These can be added to for specific formats. Note that the 'x' prefix is convention for an extended type.

Note CF-JSON is is just a specification within JSON and has standard mime type of application/json.


