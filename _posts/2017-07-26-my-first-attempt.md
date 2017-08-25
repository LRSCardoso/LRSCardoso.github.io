---
layout: post
title:  "first attempt"
date:   2017-07-26 
categories: newbie
---

This is just my **first attempt**
<br>
Taking the idea from the Population Lines Print by <http://spatial.ly/> / Dr James Cheshire i try to replicate with a twist on Qgis & Postgis. 
<br>
The first thing was to find the data in grid with the population of the world. <br>
<http://sedac.ciesin.columbia.edu/> has a huge database very well documented.
With the data in the hands, already with the projection wgs 84/Pseudo Mercator,  I sent it to the postgis database. <br>


So the first step was basically to build a query in which maked a new latitude field where added the population value to latitude.
Then I had to define the data in clusters within proximity 15000, need this to not connect the islands to the continents as example.


Then I grouped the data by latitude (original), by country and by cluster, after that I just created a line connecting the points.


At this point I already had the world outlined with rows differentiated by latitudes, but I needed polygons, so I added in each line 3 points:

* At the beginning of the line a point with the x coordinate of the starting point and y coordinate of the original latitude,

* At the end of the line another point with the x-coordinate of the end and y-coordinate of the original latitude

* Finally, I added at the end of the line a point with the x-coordinate of the starting point of the line and the y-coordinate of the original latitude to close the polygon.
<br>
the query i made is presented below:

```
 create table "Novo".poligonos2 as (
	select row_number () over() as id, lat, st_makepolygon(st_addpoint(st_addpoint(st_addpoint(ST_Makeline(geom_2), ST_Makepoint(ST_x(st_endpoint(ST_Makeline(geom_1))),ST_y(geom_1),-1)), ST_Makepoint(ST_x(st_startpoint(ST_Makeline(geom_1))),ST_y(geom_1)),0),ST_Makepoint(ST_x(st_startpoint(ST_Makeline(geom_1))),ST_y(geom_1)),-1))
	from 
	(select *, st_x(geom_1), ST_y(geom_1), st_makepoint(st_x(geom_1), ST_y(geom_1)+population/10) as geom_2
	from 
	(select *, ST_asText(ST_GeometryN(geom,1))as geom_1
	from (select w.*, clusterid 
	from (
	select row_number() over (lat) as clusterid, *
	from (
	select unnest(ST_ClusterWithin(geom, 15000)) gc, lat 
	from "Novo"."WorldGrid_mercator" as w
	where iso_a2='PT'
	group by lat ) f ) g, "Novo"."WorldGrid_mercator" as w
	where w.geom && gc   ) as wc
	where wc."size"<>1 and iso_a2='PT' 
	) as pt) as pt2
	group by lat,cluste
  
```

<br>

![Alt Text](http://LRSCardoso.github.io/mapa_linhas_populacao_lt.jpg)

<br>
globe version 
<br>
<img src="http://LRSCardoso.github.io/mapa_linhas_populacao4.jpeg" width="700" height="700" />
