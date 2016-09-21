# Getting Started With the APIs
We must first gain permission to use the following apis:
* Google Maps Javascript API
* Google Maps Roads API
* Google Static Maps API
* Google Street View Image API
* Google Places API Web Service
* Google Maps Geocoding API
* Google Maps Directions API
* Google Maps Distance Matrix API
* Google Maps Geolocation API
* Google Maps Elevation API
* Google Maps Time Zone API

Here are the basics of how to use Google Maps:
```html
<!-- This is the corresponding "starter code" for 04_Hello Map in Udacity and Google's Maps
API Course, Lesson 1 -->
<html>
 <head>
 <!-- styles put here, but you can include a CSS file and reference it instead! -->
   <style type="text/css">
     html, body { height: 100%; margin: 0; padding: 0; }
     #map { height: 100%; }
   </style>
 </head>
 <body>
   <div id="map"></div>
   <script>
     var map;

     function initMap() {
       map = new google.maps.Map(document.getElementById('map'), {
        center: { lat: 40.7413549, lng: -73.9980244 },
        zoom: 13
       });
     }
   </script>
   <script async defer
    src="https://maps.googleapis.com/maps/api/js?key=XXXXXXXXXXXXXXX&v=3&callback=initMap">
   </script>
 </body>
</html>
```

Here is creating a marker and infowindow on Google Maps: 
```html
<!-- This is the corresponding "starter code" for 07_Markers/Infowindows in Udacity and Google's Maps
API Course, Lesson 1 -->
<html>
 <head>
 <!-- styles put here, but you can include a CSS file and reference it instead! -->
   <style type="text/css">
     html, body { height: 100%; margin: 0; padding: 0; }
     #map { height: 100%; }
   </style>
 </head>
 <body>
   <div id="map"></div>
   <script type="text/javascript">
     // Create a map variable
     var map;
     // Function to initialize the map within the map div
     function initMap() {
       map = new google.maps.Map(document.getElementById('map'), {
         center: {lat: 40.74135, lng: -73.99802},
         zoom: 14
       });
       // Create a single latLng literal object.
       var singleLatLng = {lat: 40.74135, lng: -73.99802};
       // TODO: Create a single marker appearing on initialize -
       // Create it with the position of the singleLatLng,
       // on the map, and give it your own title!
       var marker = new google.maps.Marker({
        position: singleLatLng,
        map: map,
        title: 'Marker for Single LatLng Literal'
       })
       // TODO: create a single infowindow, with your own content.
       // It must appear on the marker
       var infowindow = new google.maps.InfoWindow({
        content: "Some information about the marker"
       });

       // TODO: create an EVENT LISTENER so that the infowindow opens when
       // the marker is clicked!
       marker.addListener('click', function() {
        infowindow.open(map, marker);
       });
     }
   </script>
   <!--TODO: Insert your API Key in the below call to load the API.-->
   <script async defer
     src="https://maps.googleapis.com/maps/api/js?v=3&key=XXXXXXXXXXXXX&callback=initMap">
   </script>
 </body>
</html>

```