---
title: ""
date: 2020-03-20T20:33:03+08:00
type: "page"
draft: false
---

<!-- HTML -->
<div id="travelmap"></div>

<!-- Styles -->
<style>
#travelmap {
  width: 100%;
  height: 500px;
}
.amcharts-chart-div a {
  display: none !important;
}
</style>

<!-- Resources -->
<script src="https://www.amcharts.com/lib/3/ammap.js"></script>
<script src="https://www.amcharts.com/lib/3/maps/js/worldLow.js"></script>
<script src="https://www.amcharts.com/lib/3/maps/js/worldHigh.js"></script>
<script src="https://www.amcharts.com/lib/3/themes/light.js"></script>

<!-- Chart code -->
<script>
/**
 * Define SVG path for target icon
 */
var targetSVG = "M9,0C4.029,0,0,4.029,0,9s4.029,9,9,9s9-4.029,9-9S13.971,0,9,0z M9,15.93 c-3.83,0-6.93-3.1-6.93-6.93S5.17,2.07,9,2.07s6.93,3.1,6.93,6.93S12.83,15.93,9,15.93 M12.5,9c0,1.933-1.567,3.5-3.5,3.5S5.5,10.933,5.5,9S7.067,5.5,9,5.5 S12.5,7.067,12.5,9z";

/**
 * Create the cities
 * https://www.latlong.net
 */
const cities = [
  {
    "title": "北京",
    "latitude": 39.90419989999999,
    "longitude": 116.4073963,
  },
  {
    "title": "上海",
    "latitude": 31.2240453,
    "longitude": 121.1965663,
  },
  {
    "title": "成都",
    "latitude": 30.655806,
    "longitude": 104.077000,
  },
  {
    "title": "内蒙古",
    "latitude": 38.8177603,
    "longitude": 105.454985,
  },
  {
    "title": "银川",
    "latitude": 38.4557421,
    "longitude": 106.1052623,
  },
  {
    "title": "青岛",
    "latitude": 36.0620368,
    "longitude": 120.3825681,
  },
  {
    "title": "杭州",
    "latitude": 30.2616125,
    "longitude": 120.0318058,
  },
  {
    "title": "绍兴",
    "latitude": 29.9929308,
    "longitude": 120.5176462,
  },
  {
    "title": "曼谷",
    "latitude": 13.7248936,
    "longitude": 100.4930254,
  },
  {
    "title": "普吉岛",
    "latitude": 7.8309254,
    "longitude": 98.0797376,
  },
];

for (var i = 0; i < cities.length; ++i){
  cities[i]["zoomLevel"] = 5;
  cities[i]["scale"] = 0.5;
  cities[i]["svgPath"] = targetSVG;
  cities[i]["scale"] = 0.5;
}

/**
 * Create the map
 * @https://zh.wikipedia.org/wiki/%E5%9C%8B%E5%AE%B6%E5%9C%B0%E5%8D%80%E4%BB%A3%E7%A2%BC#%E5%88%97%E8%A1%A8
 */
var map = AmCharts.makeChart( "travelmap", {
  "type": "map",
  "projection": "mercator",
  "theme": "light",
  "imagesSettings": {
    "rollOverColor": "#089282",
    "rollOverScale": 3,
    "selectedScale": 3,
    "selectedColor": "#089282",
    "color": "#13564e",
  },
  "areasSettings": {
    autoZoom: true,
    "unlistedAreasColor": "#15A892",
    "outlineThickness": 1,
    "color": "#B4B4B7",
    "colorSolid": "#84ADE9",
    "selectedColor": "#84ADE9",
    "outlineColor": "#666666",
    "rollOverColor": "#9EC2F7",
    "rollOverOutlineColor": "#000000"
  },
  "dataProvider": {
    "map": "worldHigh",
    getAreasFromMap: true,
    "images": cities,
    areas: [
      {
          "id": "CN",
          "showAsSelected": true
      },
      {
          "id": "TH",
          "showAsSelected": true
      },
    ],
  },
  "export": {
    "enabled": false,
  },
} );

</script>
