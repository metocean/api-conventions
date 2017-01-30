# api-conventions
# Query components
This follows http://www.opensearch.org/Specifications/OpenSearch/Extensions/Geo/1.0/Draft_2 with some extensions. Ordering is consistent with WMS. Longitudes must be in the interval -180.<lon<=180.
##Time selection
###Time range
Select by time range
```
time={ISO8601 time range with double dash option}
E.g.
time=2007-03-01T13:00:00Z--2008-05-11T15:30:00Z
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

##Geographical extraction/interpolation
###Point
Interpolate onto single point
```
lon={decimal longitude}&lat={decimal latitiude}
E.g.:
lon=165.5&lat=-34.5
```

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
