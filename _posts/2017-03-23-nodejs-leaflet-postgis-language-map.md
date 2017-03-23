---
layout: post
title: A NodeJS Leaflet Language Map Using PostGIS
categories:
- blog
---

##### My (unfinished) recreation of ... 
## the First Peoples' Language Map of BC: http://fplangmap.maakusii.com/

There's a more robust and official map commissioned by the First Peoples’ Heritage, Language and Culture Council. I'm just borrowing the data and recreating it using a different tech stack. You can view their version of the map at http://maps.fphlcc.ca/. I thought this data was great, and felt like would is a way to hack this for more utility in the future, if it were available in a simple, dependency free format. I also saw the opportunity to get exposure to some new technologies. My goal was to use NodeJS and build it as a single-page app.

I wanted to learn how to use Leaflet in NodeJS, and also gain some exposure to postGIS. I have a fairly solid Relational Database background, but not as much for GIS data specifically. I went to work deploying and configuring a postGIS database through Amazon Web Services.

The mapped data isn't immediately available, you have to do a little picking through the source. Here, the browser dev tools revealed the original source map to be using KML files to represent the data, so I used [mapbox/togeojson](https://github.com/mapbox/togeojson) to make geoJSON files out of these. The results went into the database.

I used express-generator to build, and put the leaflet component in index.js Locally, this ran fine, but always broke when I pushed the code to a Heroku app. After a while debugging and refactoring, I realized what it was. It came down to AWS security permissions blocking heroku from accessing it. AWS securities are managed through the EC2 console, and I just opened it up. The next problem would be hiding my credentials, but I will just put the config JSON into a gitignore file before making the source public. Finally, when it was finally finished and working on Heroku, I configured the DNS to point to my own domain. I'll be cleaning this up and prettifying the interface over the next little while.

I'm quite happy about how it came out, even though I'm always a little unimpressed by mapping apps, unless they're functioning at a Google level of quality and depth. Still, it wasn't bad for a first foray.

---
> * Essential information came from the MIT Department of Urban Studies and Planning [Intro to PostGIS](http://duspviz.mit.edu/tutorials/intro-postgis/) and [Web Mapping workshop IX](http://duspviz.mit.edu/web-map-workshop/leaflet_nodejs_postgis/). They have great resources there, so check them out.
* Thanks First Peoples’ Heritage, Language and Culture Council for your data and inspiration.