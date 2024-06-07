# Overture Data Analysis Report

## Release Used

- **Overture release**: 2024-05-16-beta.0

## Objectives

### Primary Objective
- To perform qualitative and quantitative analysis of Overture map data.

### Secondary Objectives
- Visualize the releases on a country level.
- Conduct qualitative analysis to identify additions to existing OSM data and differences across countries.
- Facilitate general users in forming their own opinions based on the available data.

## Approach

1. Build a script to retrieve Overture data as geoparquet with multiple themes (streamlining and automating the process).
2. Convert geoparquet to geojson.
3. Convert flattened geojson to pmtiles.
4. Develop a viewer for comparison and loading.
5. Automate the entire process with a bash script.
6. Compare with population data, existing OSM buildings in the area, and if possible, the number of people per building.

## Considerations

- Duckdb, overturemaps-py, and GDAL were tested for extraction, with overturemaps-py standing out as simple and perfect. The repo was forked, and enhancements were added to the viewer and filters to support any custom key and value.
- Tippecanoe was used to convert geojsonseq to pmtiles.
- A bash script was used to automate the entire process, making it configurable using config.json ([base](https://github.com/kshitijrajsharma/overture-to-tiles/blob/master/scripts/base_theme.json) and [default](https://github.com/kshitijrajsharma/overture-to-tiles/blob/master/scripts/default_theme.json)) for layers, their properties, tile generation settings, combining multiple layers into a single tile, and fetching the right key and value for specific layers.
- The primary statement being validated is: "Overture Maps data will undergo validation checks to detect map errors, breakage, and vandalism to help ensure that map data can be used in production systems."

  ![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/363fbb4f-8a46-4e86-b28e-0a0bad12dbc3)

## Study Areas 

- Argentina
- Indonesia & Malaysia Area
- Kenya
- Liberia
- Malawi
- Nepal
- Nigeria

Note: Covering bounding boxes were drawn to somehow match the country boundary in above listed countries ( this is not true for all of them - actual boundary may differ ). Data on those bbox were downloaded, viewed, analyzed, and compared regarding its distribution and how it fits with the existing population.

![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/7e038bdd-082d-4c8d-a310-5ff1bc350f8d)

View Geojson [Here](data/study-area.geojson)

## Qualitative Analysis

### Roads
- Roads are not cleaned and validated.

  ![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/adcb947e-4cf4-45ea-bf5e-2199c4332c4b)
- When a release is published, there are no major enhancements, and orphan roads remain in the datasets.
- Tags are not fixed or validated (For eg: In Nepal, most of the roads were classified as unclassified - same as OSM. Some major roads have inconsistency in trunk and primary). It appears that tags validation is still ongoing or something is not being looked into.

### Buildings
- Buildings seem to have undergone good conflation.
- Offset and merging of ML datasets have been taken care of.
- Buildings present on satellite images seem to be included in the dataset.

  ![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/9d2c6e96-2905-4b80-8016-e2fe8e7378f9)

  ![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/c18fa3b2-cad6-4d21-9f8e-eba76ff9dcbf)

### Validation Issues
- Pular Pisau, Borneo (Near Malaysia):

  ![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/665d06de-567d-4b80-b869-42116a77d4ca)

 - Height feature is present in only some buildings. In countries like Nepal, it is minimal.

   ![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/7a685baa-2a0f-4d5e-babf-e775f1b8bbc0)

- POI datasets appear to be detailed and populated in most places, making them easily importable into OSM.

  ![image](https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/dc9e7168-d2cf-4601-adc8-e6d34ff6f135)

## Quick summary 
- Overture datasets stand out well for building footprints and POIs, relatively speaking. Transportation, Land, and Land Use seem somewhat similar to OpenStreetMap.
- Validation and conflation are poor in layers other than buildings.
- Good offset alignment with roads.

## Quantitative Analysis

| Area      | Google Open Buildings | %    | Microsoft ML Buildings | %    | OpenStreetMap (as per Overture info) | %   | Total Overture Buildings | Population Estimate | P.E. (in mil) | People per Building | Approx Current OSM Buildings |
|-----------|-----------------------|------|------------------------|------|-------------------------------------|------|--------------------------|---------------------|----------------|---------------------|------------------------------|
| Argentina | 34,545,592            | 73%  | 8,998,855              | 19%  | 3,457,499                          | 7%   | 47,001,946               | 78,765,589          | 78.77          | 1.68               | 3,497,866                    |
| Liberia   | 1,557,014             | 55%  | 144,185                | 5%   | 1,148,863                          | 40%  | 2,850,062                | 10,157,546          | 10.16          | 3.56               | 1,151,027                    |
| Indonesia | 4,314,085             | 41%  | 2,485,377              | 24%  | 3,641,263                          | 35%  | 10,440,725               | 27,523,228          | 27.52          | 2.64               | 3,651,924                    |
| Nepal     | 26,280,737            | 68%  | 4,396,928              | 11%  | 8,078,311                          | 21%  | 38,755,976               | 129,874,888         | 129.87         | 3.35               | 8,243,272                    |
| Malawi    | 8,882,648             | 61%  | 1,758,044              | 12%  | 3,927,989                          | 27%  | 14,568,681               | 29,256,446          | 29.26          | 2.01               | 3,943,125                    |
| Kenya     | 20,334,091            | 59%  | 3,734,399              | 11%  | 10,414,457                         | 30%  | 34,482,947               | 75,320,339          | 75.32          | 2.18               | 10,557,014                   |
| Nigeria   | 50,787,453            | 68%  | 7,150,013              | 10%  | 16,304,722                         | 22%  | 74,242,188               | 252,698,591         | 252.70         | 3.40               | 17,966,401                   |

Overture release: 2024-05-16-beta.0

PS: Population and Current OSM Buildings Estimate is from Kontour API  
People per building =  Population Estimate on the Area  / Total Overture Buildings 
Approx current OSM buildings = Fetched from the OSM at current date to validate the overture osm building numbers may not match as overture kept snapshot of osm and by the time of this analysis buildings might increase or decrease in osm, should give rough idea  
Analysis was not done on exact country boundary, its bbox taken in the area as provided in the geojson and shared the same geometry using different parameters

## Conclusion

From the qualitative analysis conducted on different parts of the world, the data is impressive in terms of offset management when different sources are grouped. I am preetty amazed to see the coverage along with conflation and offset  accross the different parts of the world. Buildings seem to be well-matched with each other on an obsolete level, and when ground truth checking with Esri imagery, it covers most places. However, when combined with the tabular analysis in most of the places people-per-building ratio are not that realistic yet they are not worst too (seems it doesn't left out and covers most , it might have some extra clutter buildings). For example, in Argentina, it's 1.68 which seems pretty low. It appears that OpenStreetmap buildings are preserved and are as told (given highest priority - if you look into current approx osm buildings and numbers included in overture they are quite similar). A massive number of AI building footprints are added to the datasets, whereas google buildings are almost more than 50% in all of the area (Except Indonesia). For roads, validation is still poor, especially in areas like Nepal and Indonesia, where many orphan roads exist in the datasets.I expect tags validation and cleaning specially on road which is not case in the areas I looked into , tags such as primary roads , trunk , unclassified roads are inconsistents. The POI datasets seem well-detailed, and there is great potential for them to be added to OSM after validation, as Rapid already has this functionality. 3D height data is not impressive in the developing countries yet I was surprised to see some of them in countries like Nepal. Building footprints seems to be well defined and aligned with transportation layers exploring the potential that it can be quickly checked validated and used in case of pre disaster response and many other usecases.

This is my only personal view with quick analysis on the area I looked into. It is suggested to form your own opinion using the developed tools and data shared as shown in the video by the end of this blog.

## Tools and Resources Developed

### Querier 

https://queryparquet.streamlit.app/ 

### Viewer

The viewer can directly be accessed from Querier or also available here: https://kshitijrajsharma.github.io/overture-to-tiles/ 
Viewer supports remote pmtiles and custom styling , Example viewer with default styling : 

### Extractor 

https://github.com/kshitijrajsharma/overture-to-tiles/blob/master/scripts/Readme.md
https://github.com/kshitijrajsharma/overture-to-tiles/ 

### Quick demo how you can visualize and analyze the data 

https://github.com/kshitijrajsharma/overture-data-analysis-report/assets/36752999/31eb9917-3fff-42db-9f5d-d2c53649bb81

### Resources and Credits : 
- Pmtiles , Overture-py , Tippecanoe , Overture-docs , Rapid 

