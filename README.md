# Lab 1: Geolocation and mobile optimization with Leaflet
## TGIS 504, Winter 2020, Dr. Emma Slager
### Introduction
In this lab, you will practice making web maps that recognize and design for both the unique constraints of mobile devices and their unique affordances. You will create a map that places a marker on the map based on the locational information provided by the user's device, include signifiers that indicate to the user the accuracy of their device's locational information, add functionality that switches the base map between light and dark based on whether the sun is up at the user's location, and utilize various other design conventions that optimize mobile map use. This lab also asks you to read relevant documentation on the technologies used and to answer a few questions about that documentation in order to assess your understanding of its contents.

This lab is based on the tutorial [Leaflet on Mobile](https://leafletjs.com/examples/mobile), with modifications and additions by myself. 
### Set up your workspace
Begin by downloading the zipped folder of lab templates. This should contain an index.html, javascript.js, and styles.css file. Extract the files and save them to your workspace, making sure to set up an appropriate folder structure for the new term's work. Open the files in Atom or the text editor of your choice. Eventually, you will upload the files to GitHub or your UW server space, so you may wish to create a repository for your files now, which also provides the benefit of serving as a backup for your work. As always, I recommend saving your work frequently and testing it regularly using atom-live-server or a similar Atom package. We will not be using any geojson files in this lab, so you won't have cross-origin issues and local testing will therefore also be possible.

### Step 1: Prepare the page and initialize the map
In your index.html file, add the necessary links to Leaflet's CSS and JS libraries to the `head`. I recommend using a CDN rather than locally hosting the libraries: 
 ``` html
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
   integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
   crossorigin=""/>
  <script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
   integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
   crossorigin=""></script>
  ```
 In the `body` of the index, create a `div` element to hold the map: 
  ``` html
  <div id="map"></div>
  ```
  Define the height of your map container in the CSS file and give the page some additional basic styling. Because we want to maximize screen real estate for the map itself, we'll set the height and width to 100%:
  ```css
  body {
    padding: 0;
    margin: 0;
  }

html, body, #map {
  height: 100%;
  width: 100vw;
}
  ```
Now, initialize the map in the JavaScript file, setting the map to display the whole world. We'll use Mapbox tiles for the basemap. Be sure to replace `{accessToken}` with your own personal Mapbox access token:
```javascript
var map = L.map('map').fitWorld();

L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}', {
    attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
    maxZoom: 18
}).addTo(map);
```
### Step 2: Geolocation
As we've discussed in lecture, we can access a device's location using the [Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API) that is built into JavaScript. Leaflet makes accessing location even easier with its built-in [`locate` method](https://leafletjs.com/reference-1.6.0.html#map-locate). We can call this method with a single line of code, and when we do so, we can set [various options](https://leafletjs.com/reference-1.6.0.html#locate-options), such as `setView`, which, if true, recenters the map on the user's location, or `watch` which, if true, will continuously watch the device's location instead of detecting it just once. 

At the bottom of your JavaScript code, call the locate method with the following options: 
```javascript
map.locate({setView: true, maxZoom: 16});
```
Here we set the maxZoom to 16, which preserves some spatial context even if a user's device gives location information that is precise enough that the setView option could zoom in further. Save your work and test the page. If everything is working as expected, the map should recenter and zoom to your location. Note that you need to give the webpage permission to access your location information before the map can access it. 
#### A quick aside 
You may notice that the text on your map is very difficult to read when zoomed in. This has something to do with retina screen detection errors and tile size (see more in a Stack Overflow thread [here](https://stackoverflow.com/questions/37040494/street-labels-in-mapbox-tile-layer-too-small)). If this happens for you, add the following options to your tile layer where you call it with the L.tileLayer method, below where you specify attribution, maxZoom, and id:
```javascript
    tileSize: tileSize: 512,
    zoomOffset: -1,
```




