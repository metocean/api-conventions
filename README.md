# api-conventions
Specification for REST API query components

##Time selection
###Time range
Select by time range
```
time={ISO8601 time range}
E.g.
time=2007-03-01T13:00:00Z/2008-05-11T15:30:00Z
```

##Time interpolation
###Time
Interpolate for single time
time={ISO8601 time}
Example: 
```
time=2007-03-01T13:00:00Z
```
###Times
Interpolate for a series of discrete times
times={ISO8601 times}
Example: 
```
times=2007-03-01T13:00:00Z,2008-05-11T15:30:00Z
```

###Time series
Interpolate for a regularly spaced time series
Time range with interval
time={ISO8601 time range}&interval={ISO8601 duration}
Example: 
```
time=2007-03-01T13:00:00Z/2008-05-11T15:30:00Z&interval=PT3H
```

##Geographical selection
###Bounding box
Select by bounding box
###Point and radius
Select by point and radius
###Level
Select by level

##Geographical extraction/interpolation
###Point
Interpolate onto single point
lon={decimal longitude}&lat={decimal latitiude}
Example:
```
lon=165.5&lat=-34.5
```

###Series of points
Interpolate onto series of points
###Trajectory
Interpolate onto a trajectory
###Grid
Interpolate onto a grid
###Level interpolation
Interpolate onto a level
