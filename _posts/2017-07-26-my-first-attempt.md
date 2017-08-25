---
layout: post
title:  "first attempt"
date:   2017-07-26 
categories: newbie
---

This is just my **first attempt**
<br>
Taking the idea from the Population Lines Print by (http://spatial.ly/) / Dr James Cheshire i try to replicate with a twist on Qgis & Postgis 
<br>
The first thing was to find the data in grid with the population of the world. <br>
(http://sedac.ciesin.columbia.edu/) has a huge database very well documented.
With the data in the hands already with the projection wgs I sent it to the postgis database <br>

So the first step was basically to create a query in which I first added a new latitude field to which I added the population value.
Then I had to define the data in groups so as not to connect the islands to the continents

Then I grouped the data by latitude (original), by country and by cluster, so I just created a line connecting the points grouped

At this point I already had the world outlined with rows differentiated by latitudes, but I needed polygons, so I added in each line 3 points:
At the beginning of the line a point with the x coordinate of the starting point and y coordinate of the original latitude,
At the end of the line another point with the x-coordinate of the end and y-coordinate of the original latitude
Finally, I added at the end of the line a point with the x-coordinate of the starting point of the line and the y-coordinate of the original latitude to close the polygon.

<br>
![Alt Text](http://LRSCardoso.github.io/mapa_linhas_populacao_lt.jpg)
<br>
globe version 
<br>
<img src="http://LRSCardoso.github.io/mapa_linhas_populacao4.jpeg" width="700" height="700" />
