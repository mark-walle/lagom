---
layout: post
title: A NodeJS Leaflet Language Map Using PostGIS
categories:
- blog
---

###### My (unfinished) edition of ... 
# [the First Peoples' Language Map of BC!](http://fplangmap.maakusii.com/)

GitHub Repo: [/mark-walle/langmap](https://github.com/mark-walle/langmap)

There's a more robustly developed and official map commissioned by the First Peoples’ Heritage, Language and Culture Council; this is a recreation... an homage. You can view their version of the map at [maps.fphlcc.ca](http://maps.fphlcc.ca/). I'm using their data and recreating the map on a different tech stack. I thought this data was great, and felt like would is a way to hack this for more utility in the future, if it were available in a simple, dependency free format. I also saw the opportunity to get exposure to some new technologies. My goal was to use NodeJS and build it as a single-page app.

I wanted to learn how to use Leaflet in NodeJS, and also gain some exposure to postGIS. I have a fairly solid Relational Database background, but not as much for GIS data specifically. I went to work deploying and configuring a postGIS database through Amazon Web Services.

The mapped data isn't immediately available, you have to do a little picking through the source. Here, the browser dev tools revealed the original source map to be using KML files to represent the data, so I used [mapbox/togeojson](https://github.com/mapbox/togeojson) to make geoJSON files out of these. The results went into the database.

I used express-generator to build the site structure, and embedded the leaflet component as a full page app under the "/" route. Locally, this started fine, but it routinely crashed when I pushed the code to a Heroku app. After a while debugging and refactoring, I realized what it was. It came down to AWS security permissions blocking Heroku from accessing it. AWS securities are managed through the EC2 console, and I just opened it up. The next problem would be hiding my credentials, but I will just put the config JSON into a gitignore file before making the source public. Finally, when it was finally finished and working on Heroku, I configured the DNS to point to my own domain. I'll be cleaning this up and prettifying the interface over the next little while.

Here are some plans for the map over the next while:

1. Data Sanitation. On the language names in particular.
2. UI improvements. Different colours for each language, at least. Later toggle for "sleeping" languages.
3. Future layers: Display Community or Reservation hubs. More languages, try to find similar cross-country sets.

I'm quite happy about how it came out. Simple is better. Maybe too simple; but it wasn't bad for a first foray into using GIS data, spatial databases, and a mapping component in NodeJS.

> * Essential information came from the MIT Department of Urban Studies and Planning [Intro to PostGIS](http://duspviz.mit.edu/tutorials/intro-postgis/) and [Web Mapping workshop IX](http://duspviz.mit.edu/web-map-workshop/leaflet_nodejs_postgis/). They have great resources there, so check them out.
* Thanks First Peoples’ Heritage, Language and Culture Council for your data and inspiration.
