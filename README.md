# Modulation_intraparcellaire


Map.centerObject(roi, 11)

var mean_0_20 =
'<RasterSymbolizer>' +
 '<ColorMap type="ramp">' +
  '<ColorMapEntry color="#CC0000" label="3.5-4.6" opacity="1" quantity="46"/>' +
  '<ColorMapEntry color="#FF0000" label="4.6-4.9" opacity="1" quantity="49"/>' +
  '<ColorMapEntry color="#FF5500" label="4.9-5.2" opacity="1" quantity="52"/>' +
  '<ColorMapEntry color="#FFAA00" label="5.2-5.4" opacity="1" quantity="54"/>' +
  '<ColorMapEntry color="#FFFF00" label="5.4-5.5" opacity="1" quantity="55"/>' +
  '<ColorMapEntry color="#D4FF2B" label="5.5-5.6" opacity="1" quantity="56"/>' +
  '<ColorMapEntry color="#AAFF55" label="5.6-5.7" opacity="1" quantity="57"/>' +
  '<ColorMapEntry color="#80FF80" label="5.7-5.9" opacity="1" quantity="59"/>' +
  '<ColorMapEntry color="#55FFAA" label="5.9-6" opacity="1" quantity="60"/>' +
  '<ColorMapEntry color="#2BFFD5" label="6-6.2" opacity="1" quantity="62"/>' +
  '<ColorMapEntry color="#00FFFF" label="6.2-6.3" opacity="1" quantity="63"/>' +
  '<ColorMapEntry color="#00AAFF" label="6.3-6.6" opacity="1" quantity="66"/>' +
  '<ColorMapEntry color="#0055FF" label="6.6-6.8" opacity="1" quantity="68"/>' +
  '<ColorMapEntry color="#0000FF" label="6.8-7.1" opacity="1" quantity="71"/>' +
  '<ColorMapEntry color="#0000CC" label="7.1-10.5" opacity="1" quantity="76"/>' +
 '</ColorMap>' +
 '<ContrastEnhancement/>' +
'</RasterSymbolizer>';

var mean_20_50 =
'<RasterSymbolizer>' +
 '<ColorMap type="ramp">' +
  '<ColorMapEntry color="#CC0000" label="3.5-4.6" opacity="1" quantity="46"/>' +
  '<ColorMapEntry color="#FF0000" label="4.6-4.9" opacity="1" quantity="49"/>' +
  '<ColorMapEntry color="#FF5500" label="4.9-5.2" opacity="1" quantity="52"/>' +
  '<ColorMapEntry color="#FFAA00" label="5.2-5.4" opacity="1" quantity="54"/>' +
  '<ColorMapEntry color="#FFFF00" label="5.4-5.5" opacity="1" quantity="55"/>' +
  '<ColorMapEntry color="#D4FF2B" label="5.5-5.6" opacity="1" quantity="56"/>' +
  '<ColorMapEntry color="#AAFF55" label="5.6-5.7" opacity="1" quantity="57"/>' +
  '<ColorMapEntry color="#80FF80" label="5.7-5.9" opacity="1" quantity="59"/>' +
  '<ColorMapEntry color="#55FFAA" label="5.9-6" opacity="1" quantity="60"/>' +
  '<ColorMapEntry color="#2BFFD5" label="6-6.2" opacity="1" quantity="62"/>' +
  '<ColorMapEntry color="#00FFFF" label="6.2-6.3" opacity="1" quantity="63"/>' +
  '<ColorMapEntry color="#00AAFF" label="6.3-6.6" opacity="1" quantity="66"/>' +
  '<ColorMapEntry color="#0055FF" label="6.6-6.8" opacity="1" quantity="68"/>' +
  '<ColorMapEntry color="#0000FF" label="6.8-7.1" opacity="1" quantity="71"/>' +
  '<ColorMapEntry color="#0000CC" label="7.1-10.5" opacity="1" quantity="76"/>' +
 '</ColorMap>' +
 '<ContrastEnhancement/>' +
'</RasterSymbolizer>';

var stdev_0_20 =
'<RasterSymbolizer>' +
 '<ColorMap type="ramp">' +
  '<ColorMapEntry color="#fde725" label="low" opacity="1" quantity="1"/>' +
  '<ColorMapEntry color="#5dc962" label=" " opacity="1" quantity="2"/>' +
  '<ColorMapEntry color="#20908d" label=" " opacity="1" quantity="3"/>' +
  '<ColorMapEntry color="#3a528b" label=" " opacity="1" quantity="4"/>' +
  '<ColorMapEntry color="#440154" label="high" opacity="1" quantity="5"/>' +
 '</ColorMap>' +
 '<ContrastEnhancement/>' +
'</RasterSymbolizer>';

var stdev_20_50 =
'<RasterSymbolizer>' +
 '<ColorMap type="ramp">' +
  '<ColorMapEntry color="#fde725" label="low" opacity="1" quantity="1"/>' +
  '<ColorMapEntry color="#5dc962" label=" " opacity="1" quantity="2"/>' +
  '<ColorMapEntry color="#20908d" label=" " opacity="1" quantity="3"/>' +
  '<ColorMapEntry color="#3a528b" label=" " opacity="1" quantity="4"/>' +
  '<ColorMapEntry color="#440154" label="high" opacity="1" quantity="5"/>' +
 '</ColorMap>' +
 '<ContrastEnhancement/>' +
'</RasterSymbolizer>';


var palettes = require('users/gena/packages:palettes');

// Choose and define a palette
var palette = palettes.kovesi.rainbow_bgyr_35_85_c73[7].reverse();
var raw = ee.Image("ISDASOIL/Africa/v1/ph").reproject({crs: 'EPSG:4326', scale: 5}).clip(roi);
// Map.addLayer(
//     raw.select(0).sldStyle(mean_0_20), {},
//     "ph, mean visualization, 0-20 cm");
// Map.addLayer(
//     raw.select(1).sldStyle(mean_20_50), {},
//     "ph, mean visualization, 20-50 cm");
// Map.addLayer(
//     raw.select(2).sldStyle(stdev_0_20), {},
//     "ph, stdev visualization, 0-20 cm");
// Map.addLayer(
//     raw.select(3).sldStyle(stdev_20_50), {},
//     "ph, stdev visualization, 20-50 cm");

var converted = raw.divide(10);

var visualization = {min: 4, max: 8};

//Map.setCenter(25, -3, 2);
var converted = converted.focal_median(20,'circle','meters')
Map.addLayer(converted.select(0), {palette:palette}, "ph, mean, 0-20 cm");
////////////////////////////////////////

/////////////////////////lime requirement/////////////////

var palette = palettes.kovesi.rainbow_bgyr_35_85_c73[7];

var lime_requirement =   converted.multiply(-1)
var lime_requirement = lime_requirement.add(6.2).select(0)
var lime_requirement = lime_requirement.multiply(2).rename('lime_requirement')
Map.addLayer(lime_requirement, {palette:palette}, "lime_requirement");


var palette = palettes.kovesi.rainbow_bgyr_35_85_c73[7];

var viz = {palette: palette, min: 0, max: 1};

var legend = ui.Panel({
  style: {
    position: 'bottom-right',
    padding: '8px 15px'
  }
});

// Create and add the legend title.
var legendTitle = ui.Label({
  value: 'lime_requirement (tonne/ha)',
  style: {
    fontWeight: 'bold',
    fontSize: '18px',
    margin: '0 0 4px 0',
    padding: '0'
  }
});

legend.add(legendTitle);

// create the legend image
var lon = ee.Image.pixelLonLat().select('latitude');
var gradient = lon.multiply((viz.max-viz.min)/100.0).add(viz.min);
var legendImage = gradient.visualize(viz);
 
// create text on top of legend
var panel = ui.Panel({
widgets: [
ui.Label(viz['max'])
],
});
 
legend.add(panel);
 
// create thumbnail from the image
var thumbnail = ui.Thumbnail({
image: legendImage,
params: {bbox:'0,0,10,100', dimensions:'10x200'},
style: {padding: '1px', position: 'bottom-center'}
});
 
// add the thumbnail to the legend
legend.add(thumbnail);
 
// create text on top of legend
var panel = ui.Panel({
widgets: [
ui.Label(viz['min'])
],
});

legend.add(panel);
Map.add(legend);








////////////////////////////////////////////////////////////
var palette = palettes.matplotlib.viridis[7].reverse();

var nitro = ee.Image("ISDASOIL/Africa/v1/nitrogen_total").clip(roi);
var nitro = nitro.focal_median(20,'circle','meters')
Map.addLayer(
    nitro.select(0), {palette:palette},
    "Nitrogen, total, mean visualization, 0-20 cm");


///////////////
var palette = palettes.niccoli.isol[7].reverse();

var phos = ee.Image("ISDASOIL/Africa/v1/phosphorus_extractable").clip(roi);
var phos = phos.focal_median(20,'circle','meters')
Map.addLayer(
    phos.select(0), {palette:palette},
    "Phosphorus extractable, mean visualization, 0-20 cm");

////////////////////////////////////////////::
var palette = palettes.niccoli.cubicyf[7].reverse();

var potass = ee.Image("ISDASOIL/Africa/v1/potassium_extractable").clip(roi);
var potass = potass.focal_median(20,'circle','meters')

Map.addLayer(
    potass.select(0), {palette:palette},
    "Potassium extractable, mean visualization, 0-20 cm");
    //////////////////////////////////////////////
    
    
    // ==================== CALCUL DES BESOINS NETS EN N, P, K (arachide, 3 t/ha) ====================

// 1. Retraits totaux (kg/ha) pour 3 t/ha d’arachide
var target_yield = 3; // tonnes/ha
var need_N = ee.Number(30).multiply(target_yield);
var need_P2O5 = ee.Number(18.3).multiply(target_yield);
var need_K2O = ee.Number(54).multiply(target_yield);

// 2. Conversion des valeurs iSDA (approximation du kg/ha)
// Ici on applique les back-transformations selon la doc iSDA
var N_soil = nitro.expression('exp(b1/100.0)-1.0', {b1:nitro});
var P_soil = phos.expression('exp(b1/10.0)-1.0', {b1:phos});
var K_soil = potass.expression('exp(b1/10.0)-1.0', {b1:potass});

// Conversion ppm → kg/ha (0-20 cm, densité 1.3 g/cm3)
var depth_cm = 20;
var bulk_density = 1.3;
function ppm_to_kg_ha(img){ return img.multiply(depth_cm).multiply(bulk_density).divide(10); }

var N_kg_ha = ppm_to_kg_ha(N_soil);
var P2O5_kg_ha = ppm_to_kg_ha(P_soil).multiply(2.29);
var K2O_kg_ha = ppm_to_kg_ha(K_soil).multiply(1.20);

// 3. Calcul des besoins nets (si le sol couvre une partie du besoin)
var needN_net = N_kg_ha.multiply(-1).add(need_N).max(0);
var needP_net = P2O5_kg_ha.multiply(-1).add(need_P2O5).max(0);
var needK_net = K2O_kg_ha.multiply(-1).add(need_K2O).max(0);

// 4. Affichage des résultats moyens sur ton ROI

// Sélection du premier band pour chaque image
var N_band = N_kg_ha.select(0);
var P_band = P2O5_kg_ha.select(0);
var K_band = K2O_kg_ha.select(0);
var needN_band = needN_net.select(0);
var needP_band = needP_net.select(0);
var needK_band = needK_net.select(0);

// Réduction sur le ROI (calcul des moyennes)
var stats = ee.Dictionary({
  N_soil_kg_ha: N_band.reduceRegion({reducer: ee.Reducer.mean(), geometry: roi, scale: 250, maxPixels: 1e9}).values().get(0),
  P2O5_soil_kg_ha: P_band.reduceRegion({reducer: ee.Reducer.mean(), geometry: roi, scale: 250, maxPixels: 1e9}).values().get(0),
  K2O_soil_kg_ha: K_band.reduceRegion({reducer: ee.Reducer.mean(), geometry: roi, scale: 250, maxPixels: 1e9}).values().get(0),
  N_need_net_kg_ha: needN_band.reduceRegion({reducer: ee.Reducer.mean(), geometry: roi, scale: 250, maxPixels: 1e9}).values().get(0),
  P2O5_need_net_kg_ha: needP_band.reduceRegion({reducer: ee.Reducer.mean(), geometry: roi, scale: 250, maxPixels: 1e9}).values().get(0),
  K2O_need_net_kg_ha: needK_band.reduceRegion({reducer: ee.Reducer.mean(), geometry: roi, scale: 250, maxPixels: 1e9}).values().get(0)
});

print('=== BESOINS NETS EN N, P2O5, K2O POUR 3 t/ha D’ARACHIDE ===', stats);

// 5. Visualisation des besoins nets sur la carte
// 5. Visualisation des besoins nets sur la carte
Map.addLayer(needN_net.select(0), {min:0, max:100, palette:['#FDE725', // jaune vif
  '#B5DE2B',
  '#6CCE59',
  '#35B779',
  '#1F9E89',
  '#26828E',
  '#31688E',
  '#3E4989',
  '#482878',
  '#440154'  ]}, 'Besoin net N (kg/ha)')
Map.addLayer(needP_net.select(0), {min:0, max:50, palette:['#023FA5', // bleu profond
  '#7D87B9',
  '#BEC1D4',
  '#D6BCC0',
  '#BB7784',
  '#8E063B', // bordeaux
  '#4A6FE3',
  '#8595E1',
  '#B5BBE3',
  '#E6AFB9',
  '#E07B91',
  '#D33F6A',
  '#11C638',
  '#8DD593',
  '#C6DEC7',
  '#EAD3C6',
  '#F0B98D',
  '#EF9708' ]}, 'Besoin net P2O5 (kg/ha)');
Map.addLayer(needK_net.select(0), {min:0, max:150, palette:['#3F007D', // violet foncé
  '#54278F',
  '#756BB1',
  '#9E9AC8',
  '#CBC9E2',
  '#F2F0F7',
  '#FEF0D9',
  '#FDD49E',
  '#FDBB84',
  '#FC8D59',
  '#E34A33',
  '#B30000',
  '#7F0000']}, 'Besoin net K2O (kg/ha)');


////////////////////////////
var palette = palettes.colorbrewer.YlOrBr[7];

var carbon = ee.Image("ISDASOIL/Africa/v1/carbon_organic").clip(roi);
var carbon = carbon.focal_median(20,'circle','meters')

Map.addLayer(
    carbon.select(0), {palette:palette},
    "Carbon, organic, mean visualization, 0-20 cm");


var clay =  ee.Image("ISDASOIL/Africa/v1/clay_content").select(0).rename('clay').clip(roi)
var sand = ee.Image("ISDASOIL/Africa/v1/sand_content").select(0).rename('sand').clip(roi)
var silt = ee.Image("ISDASOIL/Africa/v1/silt_content").select(0).rename('silt').clip(roi)
var textur1 = clay.addBands(silt).addBands(sand)
print(textur1, 'texture')
Map.addLayer(textur1, {}, 'text')
var texture = clay.multiply(1).add(silt.multiply(1)).add(sand.multiply(1));
var texture = texture.reproject({crs: 'EPSG:4326', scale: 10})
var palette = palettes.niccoli.cubicyf[7].reverse();

// var texture = texture.reproject({crs: 'EPSG:4326', scale: 5})
var texture = texture.focal_median(20,'circle','meters')
Map.addLayer(
    texture, {palette:['cc4c02', 'eaecda', 'ffcc66']},
    "texture");

Export.image.toDrive({
  image:texture,
  description:'texture',
  region: roi,
  scale : 10,
  maxPixels: 1e9})


print(texture)

var s2 = ee.ImageCollection('COPERNICUS/S2_SR');

// Create a function to mask clouds using the Sentinel-2 QA band.
function maskS2clouds(image) {
  var qa = image.select('QA60');

  // Bits 10 and 11 are clouds and cirrus, respectively.
  var cloudBitMask = ee.Number(2).pow(10).int();
  var cirrusBitMask = ee.Number(2).pow(11).int();

  // Both flags should be set to zero, indicating clear conditions.
  var mask = qa.bitwiseAnd(cloudBitMask).eq(0).and(
             qa.bitwiseAnd(cirrusBitMask).eq(0));

  // Return the masked and scaled data.
  return image.updateMask(mask).divide(10000);
}

// Filter clouds from Sentinel-2 for a given period.
var composite = s2.filterDate('2020-07-01', '2021-10-30')
                  // Pre-filter to get less cloudy granules.
                  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
                  .map(maskS2clouds)
                  .select('B2', 'B3', 'B4','B5','B6','B7','B8','B11', 'B12'); 

// Reproject to WGS 84 UTM zone 35s                  
var S2_composite = composite.median().reproject({crs: 'EPSG:4326', scale: 2});

// Check projection information                 
print('Projection, crs, and crs_transform:', S2_composite.projection());

// Display a composite S2 imagery
//Map.addLayer(S2_composite.clip(roi), {bands: ['B11', 'B8', 'B3'], min: 0, max: 0.3});
////////////////
var ndvi = S2_composite.normalizedDifference(['B8', 'B4']).clip(roi)
Map.addLayer(ndvi, {palette:palette}, 'ndvi')


















///////////////////////:legende///////////////////
var palettes = require('users/gena/packages:palettes');

// Choose and define a palette
var palette = palettes.colorbrewer.YlOrBr[7];
var viz = {palette: palette, min: 13, max: 16};

var legend = ui.Panel({
  style: {
    position: 'bottom-left',
    padding: '8px 15px'
  }
});

// Create and add the legend title.
var legendTitle = ui.Label({
  value: 'Organic_carbon (g/kg)',
  style: {
    fontWeight: 'bold',
    fontSize: '18px',
    margin: '0 0 4px 0',
    padding: '0'
  }
});

legend.add(legendTitle);

// create the legend image
var lon = ee.Image.pixelLonLat().select('latitude');
var gradient = lon.multiply((viz.max-viz.min)/100.0).add(viz.min);
var legendImage = gradient.visualize(viz);
 
// create text on top of legend
var panel = ui.Panel({
widgets: [
ui.Label(viz['max'])
],
});
 
legend.add(panel);
 
// create thumbnail from the image
var thumbnail = ui.Thumbnail({
image: legendImage,
params: {bbox:'0,0,10,100', dimensions:'10x200'},
style: {padding: '1px', position: 'bottom-center'}
});
 
// add the thumbnail to the legend
legend.add(thumbnail);
 
// create text on top of legend
var panel = ui.Panel({
widgets: [
ui.Label(viz['min'])
],
});

legend.add(panel);
//Map.add(legend);

Map.setCenter(-16.357829, 14.507133, 11)
//Map.centerObject(roi, 11)




///////////////////////
//////////////////
// set position of panel
//////////////////
// set position of panel
var legend = ui.Panel({
  style: {
    position: 'bottom-left',
    padding: '30px 20px'
  }
});
 
// Create legend title
var legendTitle = ui.Label({
  value: 'SOIL_TEXTURE',
  style: {
    fontWeight: 'bold',
    fontSize: '15px',
    margin: '0 0 4px 0',
    padding: '0'
    }
});
 
// Add the title to the panel
legend.add(legendTitle);
 
// Creates and styles 1 row of the legend.
var makeRow = function(color, name) {
 
      // Create the label that is actually the colored box.
      var colorBox = ui.Label({
        style: {
          backgroundColor: '#' + color,
          // Use padding to give the box height and width.
          padding: '8px',
          margin: '0 0 4px 0'
        }
      });
 
      // Create the label filled with the description text.
      var description = ui.Label({
        value: name,
        style: {margin: '0 0 4px 6px'}
      });
 
      // return the panel
      return ui.Panel({
        widgets: [colorBox, description],
        layout: ui.Panel.Layout.Flow('horizontal')
      });
};
 
//  Palette with the colors
var palette =['cc4c02', 'eaecda', 'ffcc66'];
 
// name of the legend
var names = ['CLAY','SILT',
'SAND' ];
 
// Add color and and names
for (var i = 0; i < 3; i++) {
  legend.add(makeRow(palette[i], names[i]));
  }  
 
// add legend to map (alternatively you can also print the legend to the console)
Map.add(legend);
