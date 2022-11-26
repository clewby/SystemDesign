# [Geospatial Indexes](https://www.youtube.com/watch?v=VXWcNGxKMq4)

## Backgroung:
* To perform queries based on the location of a given point
* Find all objects within a certrain distance(radius of the original point(Uber, Yelp, Tinder, DoorDash)

## The problem:
* We have to perform a scan over most of the point to check manually if they  are in the circle

## Geohashing:
* Mapping map areas of a 2d palne to a single string:
* >a b c d
* >e f g h
* Each region is split into subregions, which have an additional character per depth of subregion:
* >aa ab ac ad
* >ae af ag ah
* As the length of the GeoHash increases, the size of the bounding box of the geohash decreases! 

## Finding all points within a certain distance of a given point:
* Convert original point to geohash
* Trancate geohash to proper depth (e.g. 1, 5, 10 miles)
* Use geohash index to find all of the points in the larger bounding box, and chack which ones are within the proper radius