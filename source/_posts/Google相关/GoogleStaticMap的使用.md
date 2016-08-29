title: GoogleStaticMap的使用
date: 2014-11-21 13:36:46
categories: google相关
tags: [google, Map]
---

做前端不可避免的会接触到地图服务，Google Map是面试应用中的佼佼者。它提供的功能很全面，也方便定制。<!--more-->

## Google Maps Image APIs
[Google Maps Image APIs](https://developers.google.com/maps/documentation/imageapis/), 顾名思义，就是提供静态图片的服务。可以传递坐标，尺寸等信息。

提供了两种API：
- [Static Maps](https://developers.google.com/maps/documentation/staticmaps/)
- [Street View](https://developers.google.com/maps/documentation/streetview/)

## 一个需求
昨日帮同事做个小功能，给定三个地点的经纬度以及尺寸，输出三个地点在所输出的图片上的坐标。所用的是`Static Map`API。

## 解决方案
根据给出的点获取合适的zoom级别的图片，这个方法google直接就可以用，只需要把zoom设置为auto即可。比如:
![zoom auto](https://maps.googleapis.com/maps/api/staticmap?center=40.744907,-73.997144&zoom=auto&size=350x265&maptype=roadmap&markers=color:blue%7Clabel:S%7C40.764083,-73.995767&markers=color:green%7Clabel:G%7C40.753933,-73.900254&markers=color:red%7Clabel:C%7C40.716705,-74.095413)

但解决这个需求，有几个关键的点：
- 求出正确的zoom级别
- 求出在256x256下的坐标
- 更具zoom求出对应尺寸下的坐标

在线预览如下：[jsfiddle](http://jsfiddle.net/leohxj/Lga46rx1/)

### 求出zoom levels
由于我暂时还没理解，只能直接贴出代码：

```
// getZoom.js
function getBoundsZoomLevel(points, mapDim) {
    var WORLD_DIM = { height: 256, width: 256 };
    var ZOOM_MAX = 21;

    function latRad(lat) {
        var sin = Math.sin(lat * Math.PI / 180);
        var radX2 = Math.log((1 + sin) / (1 - sin)) / 2;
        return Math.max(Math.min(radX2, Math.PI), -Math.PI) / 2;
    }

    function zoom(mapPx, worldPx, fraction) {
        return Math.floor(Math.log(mapPx / worldPx / fraction) / Math.LN2);
    }

    var ne = getNorthEast(points);
    var sw = getSouthWest(points);

    var latFraction = (latRad(ne.lat) - latRad(sw.lat)) / Math.PI;
    
    var lngDiff = ne.lng - sw.lng;
    var lngFraction = ((lngDiff < 0) ? (lngDiff + 360) : lngDiff) / 360;
    
    var latZoom = zoom(mapDim.height, WORLD_DIM.height, latFraction);
    var lngZoom = zoom(mapDim.width, WORLD_DIM.width, lngFraction);
    
    return Math.min(latZoom, lngZoom, ZOOM_MAX)-1;
}

function getBoundsCenter(points) {
    var centerX = 0;
    var centerY = 0;
    for (var i = 0, len = points.length; i < len; i++) {
        centerX += points[i].lat;
        centerY += points[i].lng;
    }

    return {
        lat: (centerX/3),
        lng: (centerY/3)
    }
}

function getNorthEast(points) {
    var neLat = points[0].lat;
    var neLng = points[0].lng;

    for (var i = 1, len = points.length; i < len; i++) {
        neLat = Math.max(neLat, points[i].lat);
        neLng = Math.max(neLng, points[i].lng);
    }
    return {
        lat: neLat,
        lng: neLng
    } 
}

function getSouthWest(points) {
    var swLat = points[0].lat;
    var swLng = points[0].lng;

    for (var i = 1, len = points.length; i < len; i++) {
        swLat = Math.min(swLat, points[i].lat);
        swLng = Math.min(swLng, points[i].lng);
    }
    return {
        lat: swLat,
        lng: swLng
    }
}

function formatPoint2Str(point) {
    return point.lat + "," + point.lng;
}
```

### 求出256x256下坐标
```
// MercatorProjection.js
var MERCATOR_RANGE = 256;

function bound(value, opt_min, opt_max) {
  if (opt_min != null) value = Math.max(value, opt_min);
  if (opt_max != null) value = Math.min(value, opt_max);
  return value;
}

function degreesToRadians(deg) {
  return deg * (Math.PI / 180);
}

function radiansToDegrees(rad) {
  return rad / (Math.PI / 180);
}

function MercatorProjection() {
  this.pixelOrigin_ = {x:MERCATOR_RANGE/2, y:MERCATOR_RANGE/2};
  this.pixelsPerLonDegree_ = MERCATOR_RANGE / 360;
  this.pixelsPerLonRadian_ = MERCATOR_RANGE / (2 * Math.PI);
};

MercatorProjection.prototype.fromLatLngToPoint = function(latLng, opt_point) {
  var me = this;

  var point = opt_point || {x:0, y:0};

  var origin = me.pixelOrigin_;
  point.x = origin.x + latLng.lng * me.pixelsPerLonDegree_;
  // NOTE(appleton): Truncating to 0.9999 effectively limits latitude to
  // 89.189.  This is about a third of a tile past the edge of the world tile.
  var siny = bound(Math.sin(degreesToRadians(latLng.lat)), -0.9999, 0.9999);
  point.y = origin.y + 0.5 * Math.log((1 + siny) / (1 - siny)) * -me.pixelsPerLonRadian_;
  return point;
};

MercatorProjection.prototype.fromPointToLatLng = function(point) {
  var me = this;

  var origin = me.pixelOrigin_;
  var lng = (point.x - origin.x) / me.pixelsPerLonDegree_;
  var latRadians = (point.y - origin.y) / -me.pixelsPerLonRadian_;
  var lat = radiansToDegrees(2 * Math.atan(Math.exp(latRadians)) - Math.PI / 2);
  return new google.maps.LatLng(lat, lng);
};

//pixelCoordinate = worldCoordinate * Math.pow(2,zoomLevel)
```

### 求出对应指定尺寸下的坐标
```
function getCorners(center,point,zoom,mapWidth,mapHeight){
    var scale = Math.pow(2,zoom);
    var centerPx = proj.fromLatLngToPoint(center);
    var point = proj.fromLatLngToPoint(point);

    var result = {x:(point.x-centerPx.x)*scale+mapWidth/2,y:(-centerPx.y+point.y)*scale+mapHeight/2}

    return result;
    }
```


## 参考资料
- [jsfiddle: calc zoom](http://jsfiddle.net/john_s/BHHs8/6/)
- [Google Maps V3 - How to calculate the zoom level for a given bounds](http://stackoverflow.com/questions/6048975/google-maps-v3-how-to-calculate-the-zoom-level-for-a-given-bounds)
- [How to get bounds of a google static map?](http://stackoverflow.com/questions/12507274/how-to-get-bounds-of-a-google-static-map)

