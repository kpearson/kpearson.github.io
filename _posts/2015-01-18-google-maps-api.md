---
layout: post
title: Google maps api 
excerpt: "Google maps api."
tags: [intro, beginner, api, google, maps]
---
##Google Maps JavaScript API v3

###API key  
Start by aquireing a key by going to the [google developers console], create a new project 

To use the api call in development simply leave out the api key param.

##Snazzymaps
Using these styles in your website is as easy as copying the JSON on any style page.  


```ruby
var styles = [
  {
    stylers: [
      { hue: "#00ffe6" },
      { saturation: -20 }
    ]
  },{
    featureType: "road",
    elementType: "geometry",
    stylers: [
      { lightness: 100 },
      { visibility: "simplified" }
    ]
  },{
    featureType: "road",
    elementType: "labels",
    stylers: [
      { visibility: "off" }
    ]
  }
];

map.setOptions({styles: styles});
```
[Google Maps JavaScript API v3 tutroial]: https://developers.google.com/maps/documentation/javascript/tutorial
[google developers console]: https://console.developers.google.com/project
[Snazzymaps]: https://snazzymaps.com
[Google maps]: https://developers.google.com/maps/web/
