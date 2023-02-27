# Linbing1065.github.io
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> 
<html xmlns="http://www.w3.org/1999/xhtml"> 
<head> 
<title></title> 
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> 
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.0/dist/leaflet.css" />
<link rel="stylesheet" href="../src/leaflet-search.css" />
<link rel="stylesheet" href="style.css" />
</head>

<body>

<h4>CO2 emission of counties in Guangdong from 2008 to 2017 <em> (Unit: milliion ton)</em></h4>
<div id="map"></div>

<div id="post-it">
<b>Search county name:</b> <br>
 Liwan District, Yuexiu District, Huadu District...
</div>

<script src="https://unpkg.com/leaflet@1.3.0/dist/leaflet.js"></script>
<script src="../src/leaflet-search.js"></script>
<script src="data/guangdong.js"></script>
<script>

	//sample data values define in guangdong.js
	var data = guangdong_county;

	var map = new L.Map('map', {zoom: 7, center: new L.latLng([23.02, 113.75]) });

	map.addLayer(new L.TileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'));	//base layer

	var featuresLayer = new L.GeoJSON(data, {
			style: function(feature) {
				return {color: feature.properties.color };
			},
			onEachFeature: function(feature, marker) {
				marker.bindPopup('<h4 style="color:'+feature.properties.color+'">'+ feature.properties.county_name + 
					'<br> code : ' + feature.properties.county_code_x + 
					'<br> city : ' + feature.properties.city_name +
					'<br> area (km2) : ' + feature.properties.area_sqkm +				
					'<br> CO2_2008  : ' + feature.properties.co2_2008 +
					'<br> CO2_2009  : ' + feature.properties.co2_2009 +
					'<br> CO2_2010  : ' + feature.properties.co2_2010 +
					'<br> CO2_2011  : ' + feature.properties.co2_2011 +
					'<br> CO2_2012  : ' + feature.properties.co2_2012 +
					'<br> CO2_2013  : ' + feature.properties.co2_2013 +
					'<br> CO2_2014  : ' + feature.properties.co2_2014 +
					'<br> CO2_2015  : ' + feature.properties.co2_2015 +
					'<br> CO2_2016  : ' + feature.properties.co2_2016 +
					'<br> CO2_2017  : ' + feature.properties.co2_2017 +
					'<br> Average CO2 : ' + feature.properties.avg_co2 +'</h4>');
			}
		});

	map.addLayer(featuresLayer);

	var searchControl = new L.Control.Search({
		layer: featuresLayer,
		propertyName: 'county_name',
		marker: false,
		moveToLocation: function(latlng, title, map) {
			//map.fitBounds( latlng.layer.getBounds() );
			var zoom = map.getBoundsZoom(latlng.layer.getBounds());
  			map.setView(latlng, zoom); // access the zoom
		}
	});

	searchControl.on('search:locationfound', function(e) {
		
		//console.log('search:locationfound', );

		//map.removeLayer(this._markerSearch)

		e.layer.setStyle({fillColor: '#3f0', color: '#0f0'});
		if(e.layer._popup)
			e.layer.openPopup();

	}).on('search:collapsed', function(e) {

		featuresLayer.eachLayer(function(layer) {	//restore feature color
			featuresLayer.resetStyle(layer);
		});	
	});
	
	map.addControl( searchControl );  //inizialize search control

</script>

<script type="text/javascript" src="/labs-common.js"></script>

</body>
</html>
