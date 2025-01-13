# 2 Data Inventory & Sources

Mapping forest changes over time requires combining multiple Earth observation datasets, each offering unique perspectives on forest structure, phenology, and anthropogenic pressures. These datasets might vary in spatial resolution, temporal coverage, and thematic detail. Before any modeling or analysis, a clear understanding of what datasets are available—and how they complement each other—is critical. 

In this chapter, we list out the primary data layers that will form the basis of our annual forest classification. We also describe their original sources, spatial/temporal resolutions, and the key attributes that make them valuable for our forest monitoring objectives.

## 2.1 Data List Summary

All the data below has been harmonized into 100m spatial resolution,  to make it easier to calculate the areas (1 pixel = 1 hectare).

1. **National/Regional Land Cover Products**  
   - **BNETD Land Cover (2020)**: Baseline map of major land-use/land-cover classes, including multiple forest types and other land uses.

2. **Global Forest Classification & Management Data**  
   - **GFM Forest Management Types (2015)**: Information on whether forests are managed (e.g., logging, plantations) or naturally regenerating.  
   - **Global IPCC Forest Classification (2020)**: Tier-1 classification separating primary, young secondary, and old secondary forests.

3. **Forest Cover & Canopy Metrics**  
   - **Global PALSAR Forest Map (2017–2020)**: Distinguishes dense forest, non-dense forest, non-forest, and water.  
   - **JRC Forest Cover (2020), Subtypes (2020)**: Basic forest vs. non-forest layer, plus a subtype map indicating naturally regenerating vs. planted forests.  
   - **NASA GFCC (2000, 2005, 2010, 2015)**: Tree canopy cover at 30m resolution, used to track longer-term forest changes.  
   - **Hansen Global Forest Change (2000–2023)**: Tree cover (year 2000) and annual forest loss, widely used for gross deforestation estimates.  
   - **UMD/GLAD Datasets**:  
     - **Primary Humid Tropical Forests (2001)**: Presence/absence of intact humid tropical forest.  
     - **Global Forest Canopy Height (2019; 2000 & 2020)**: Canopy height layers for assessing forest structure and extent of loss/gain.  

4. **Digital Elevation & Terrain**  
   - **Copernicus DEM GLO-30**: Elevation, slope, aspect, and derived metrics at ~30m resolution. Useful for distinguishing forest types influenced by terrain.

5. **High-Resolution & Regional Products**  
   - **Meta/WRI Canopy Height (2020)**: Detailed canopy height, potentially at fine scale (1m).  
   - **ETH Sentinel-2 Canopy Height (2020)**: 10m canopy height dataset.

6. **Other Vegetation & Land-Cover Time Series**  
   - **Planet Africa Tree Cover (2019)**: High-resolution tree cover across Africa.  
   - **MODIS Land Cover (MCD12Q1, 2001–2023)**: Multiple classification schemes (IGBP, UMD, LAI, etc.), at 500m resolution.  
   - **Landsat Annual NDVI (1984–2024)**: Yearly normalized difference vegetation index composites at 30m.  
   - **MODIS VCF (2000–2020)**: Vegetation Continuous Fields (percent tree, percent non-tree vegetation, etc.).  
   - **MODIS Land Cover Dynamics (MCD12Q2, 2001–2023)**: Phenological metrics such as green-up, peak, and senescence dates.  

7. **Primary Productivity & Climate Data**  
   - **MODIS LAI/FPAR (MCD15A3H, 2003–2023)**: Leaf Area Index and Fraction of Photosynthetically Active Radiation.  
   - **MODIS GPP/NPP (MOD17A3HGF & MYD17A3HGF, 2001–2023)**: Gross and Net Primary Production estimates at 500m resolution.

8. **Historical & Annual Forest Change**  
   - **JRC TMF Annual Changes (1990–2023)**: Classification of undisturbed vs. degraded tropical moist forest, deforestation, regrowth.  
   - **Global 30m Land Cover Change (GLC_FCS30D, 1985–2022)**: Five-year maps (1985, 1990, 1995) and yearly maps (2000–2022) covering various land-cover classes.

This summary provides an at-a-glance reference for each dataset. In the following **Section 2.2**, we will detail the classification legends, spatial resolutions, file naming conventions, and references for these products.

## 2.2 Detailed Data Descriptions

The data follows a standardized naming convention in the format: 

`{iso3}_{data_source}_{name_of_variable}_{year_in_yyyy_format}.tif `

where:
- `iso3` represents the country code (e.g., CIV for Côte d'Ivoire), 
- `data_source` indicates the organization or project that produced the data (e.g., BNETD, GLAD), 
- `name_of_variable` describes the type of data (e.g., LandCover, ForestExtentGain), and 
- `year_in_yyyy_format` specifies the temporal coverage. 

When comparing changes between two time periods, the year component may include a range in the format `{yyyy}_{yyyy}`. For instance, `CIV_GLAD_ForestExtentGain_2000_2020.tif` represents data showing forest extent gains in Côte d'Ivoire between 2000 and 2020, comparing the forest extent between these two years.

1. **BNETD Land Cover**

    Available for year 2020, the pixel has value and translated into below information: 
    
    | Value | Description |
    |--------|-------------|
    | 1 | Dense forest (Forêt dense) |
    | 2 | Light forest (Forêt claire) |
    | 3 | Forest gallery (Forêt galerie) |
    | 4 | Secondary forest/degraded forest (Forêt secondaire/forêt dégradée) |
    | 5 | Mangrove |
    | 6 | Forest plantation/Reforestation (Plantation forestière/Reboisement) |
    | 7 | Swamp forest/Forest on hydromorphic soil (Forêt marécageuse/Forêt sur sol hydromorphe) |
    | 8 | Coffee Plantation (Plantation de Café) |
    | 9 | Cocoa Plantation (Plantation de Cacao) |
    | 10 | Rubber plantation (Plantation d'Hévéa) |
    | 11 | Oil palm plantation (Plantation de Palmier à huile) |
    | 12 | Coconut Plantation (Plantation de Coco) |
    | 13 | Cashew plantation (Plantation d'Anacarde) |
    | 14 | Fruit plantation / Arboriculture (Plantation fruitière / Arboricultures) |
    | 15 | Agricultural development/Other crops/Orchards/Fallow land (Aménagement agricole/Autres cultures/Vergers/Jachères) |
    | 16 | Tree savannah (Savane arborée) |
    | 17 | Shrub formations/ Thickets (Formations arbustives/ Fourrés) |
    | 18 | Herbaceous formations (Formations herbacées) |
    | 19 | Body of water, Courses and waterways (Plan d'eau, Cours et voies deau) |
    | 20 | Swampy area (Zone marécageuse) |
    | 21 | Human habitat, Infrastructure (Habitat humain, Infrastructures) |
    | 22 | Rocky outcrop (Affleurement rocheux) |
    | 23 | Bare ground (Sol nu) |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/BNETD_land_cover_v1
    Reference: https://africageoportal.maps.arcgis.com/home/user.html?user=bnetdcignCI
    Filename: 
    - `CIV_BNETD_LandCover_2020.tif`

2. **Global Forest Management Dataset**

    Available for year 2015, the pixel has value and translated into below information:
    
    | Value | Description |
    |--------|-------------|
    | 0 | Naturally regenerating forest without any signs of management, including primary forests |
    | 1 | Naturally regenerating forest with signs of management, e.g., logging, clear cuts etc |
    | 2 | Planted forests |
    | 3 | Plantation forests (rotation time up to 15 years) |
    | 4 | Oil palm plantations |
    | 5 | Agroforestry |
    
    Data Reference: https://gee-community-catalog.org/projects/gfm_100/
    Reference: https://www.nature.com/articles/s41597-022-01332-3
    Filename: 
    - `CIV_GFM_ForestManagementTypes_2015.tif`

3. **Global Forest Classification for IPCC Aboveground Biomass Tier 1 Estimates, V1**

    Available for year 2020, the pixel has value and translated into below information:
    
    | Value | Description |
    |--------|-------------|
    | 1 | Primary Forest |
    | 2 | Young Secondary Forest |
    | 3 | Old Secondary Forest |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/NASA_ORNL_global_forest_classification_2020_V1
    Reference: https://doi.org/10.1038/s41597-024-03930-9
    Filename: 
    - `CIV_ORNL_ForestTypes_2020.tif`

4. **Global 4-class PALSAR-2/PALSAR Forest/Non-Forest Map**

    Available for year 2017-2020, the pixel has value and translated into below information:
    
    | Value | Description |
    |--------|-------------|
    | 1 | Dense Forest |
    | 2 | Non-dense Forest |
    | 3 | Non-Forest |
    | 4 | Water |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/JAXA_ALOS_PALSAR_YEARLY_FNF4
    Reference: https://doi.org/10.1016/j.rse.2014.04.014
    Filename: 
    - `CIV_JAXAFNF_ForestCover_{yyyy}.tif` where yyyy = 2017-2021

5. **EC JRC global map of forest cover 2020, V2**

    Available for year 2020, the pixel has value and translated into below information:
    
    | Value | Description |
    |--------|-------------|
    | 1 | Forest |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/JRC_GFC2020_V2
    Reference: http://data.europa.eu/89h/e554d6fb-6340-45d5-9309-332337e5bc26
    Filename: 
    - `CIV_JRC_ForestCover_2020.tif`

6. **EC JRC global map of forest types 2020**

    Available for year 2020, the pixel has value and translated into below information:
    
    | Value | Description |
    |--------|-------------|
    | 1 | Naturally regenerating forest |
    | 10 | Primary forest |
    | 20 | Planted/Plantation forest |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/JRC_GFC2020_subtypes_V0
    Reference: http://data.europa.eu/89h/037ca376-ba92-49db-a8f7-0c277c1e5436
    Filename: 
    - `CIV_JRC_ForestSubtypes_2020.tif`

7. **NASA Global Forest Cover Change (GFCC) Tree Cover Multi-Year Global 30m**

    Available for year 2000, 2005, 2010, 2015, the pixel has percentage (%) value and translated into below information: 
    
    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | tree_canopy_cover | % | 0  | 100  | The percentage of pixel area covered by trees |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/NASA_MEASURES_GFCC_TC_v3
    Reference: https://doi.org/10.1080/17538947.2013.786146
    Filename: 
    - `CIV_NASAGFCC_TreeCanopyCover_{yyyy}.tif` where yyyy = 2000, 2005, 2010, 2015

8. **Hansen Global Forest Change v1.11 (2000-2023)**

    Tree Cover available for year 2000, the pixel has percentage (%) value, and Loss year available for year 2001-2023, the pixel has value equal to year (Encoded as either 0 (no loss) or else a value in the range 1-23, representing loss detected primarily in the year 2001-2023, respectively) and translated into below information:
    
    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | treecover2000 | % | 0  | 100  | Tree canopy cover for year 2000, defined as canopy closure for all vegetation taller than 5m in height. |
    | lossyear |  | 0  | 23  | Year of gross forest cover loss event |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/UMD_hansen_global_forest_change_2023_v1_11#bands
    Reference: https://doi.org/10.1126/science.1244693
    Filename: 
    - `CIV_Hansen_TreeCover_2000.tif`
    - `CIV_Hansen_ForestLoss_{yyyy}.tif` where yyyy = 2001-2023

9. **UMD/GLAD Primary Humid Tropical Forests**

    Available for year 2001, the pixel has value and translated into below information: 
    
    | Value | Description |
    |--------|-------------|
    | 1 | Primary Humid Tropical Forests |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/UMD_GLAD_PRIMARY_HUMID_TROPICAL_FORESTS_v1
    Reference: https://doi.org/10.1088/1748-9326/aacd1c
    Filename: 
    - `CIV_GLAD_PrimaryHumidTropicalForest_2001.tif`

10. **UMD/GLAD Global Forest Canopy Height, 2019**

    Available for year 2019, the pixel has value and translated into below information: 
    
    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | forest_canopy_height | meters | 0  | 50  | Forest Canopy Height. |

    Data Reference: https://glad.umd.edu/dataset/gedi/
    Reference: https://doi.org/10.1016/j.rse.2020.112165
    Filename: 
    - `CIV_GLADGEDI_ForestCanopyHeight_2019.tif`

11. **UMD/GLAD Global Forest Canopy Height, 2000 and 2020**

    Some layers are available for year 2000 and 2020, the pixel has value and translated into below information: 
    
    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | Forest_height_2000 | meters | 0  | 50  | ForestHeight_2000. |
    | Forest_height_netgain | meters | 0 | 50 | ForestHeightGain_2000_2020. |
    | Forest_stable | meters | 0 | 50 | StableForestExtent_2000_2020. |
    | Forest_gain | meters | 0 | 50 | ForestExtentGain_2000_2020. |
    | Forest_height_2020 | meters | 0 | 50 | ForestHeight_2020. |
    | Forest_height_netloss | meters | 0 | 50 | ForestHeightLoss_2000_2020. |
    | Forest_height_loss | meters | 0 | 50 |  ForestExtentLoss_2000_2020. |

    For the forest dynamic is available for year 2000 and 2020, the pixel has value and translated into below information: 
    
    | Value | Description | Year |
    |----|----|----|
    | 1 | Stable forest extent | 2000-2020 |
    | 2 | Forest extent loss | 2001-2020 |
    | 3 | Forest extent gain | 2001-2020 |
    | 4 | Forests affected by stand-replacement disturbances or degradation | 2001-2020 |
    
    Data Reference: https://glad.geog.umd.edu/dataset/GLCLUC2020
    Reference: https://doi.org/10.3389/frsen.2022.856903
    Filename: 
    - `CIV_GLAD_ForestHeight_2000.tif`
    - `CIV_GLAD_ForestHeight_2020.tif`
    - `CIV_GLAD_ForestExtentGain_2000_2020.tif`
    - `CIV_GLAD_ForestExtentLoss_2000_2020.tif`
    - `CIV_GLAD_ForestHeightGain_2000_2020.tif`
    - `CIV_GLAD_ForestHeightLoss_2000_2020.tif`
    - `CIV_GLAD_ForestDynamicType_2000_2020.tif`

12. **Copernicus DEM GLO-30: Global 30m Digital Elevation Model**

    The pixel has value and translated into below information: 
    
    | Name | Description | Unit |
    |----|----|----|
    | DEM | Digital Surface Model | meter |
    | SlopeD | Slope | Degrees |
    | SlopeP | Slope | Percent |
    | Aspect | The orientation of slope | Clockwise in degrees from 0 to 360 |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_DEM_GLO30
    Reference: https://docs.sentinel-hub.com/api/latest/static/files/data/dem/resources/license/License-COPDEM-30.pdf
    Filename: 
    - `CIV_CopDEM_Elevation.tif`
    - `CIV_CopDEM_SlopeDegrees.tif`
    - `CIV_CopDEM_SlopePercent.tif`
    - `CIV_CopDEM_Aspect.tif`

13. **Meta/WRI High Resolution 1m Global Canopy Height Maps**

    Available as average of Canopy Height for year between 2018 and 2020, the pixel has value and translated into below information: 
    
    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | canopy_height | meters | 0  | 50  | ForestHeight_2020. |

    Data Reference: https://gee-community-catalog.org/projects/meta_trees/?h=canopy
    Reference: https://www.sciencedirect.com/science/article/pii/S003442572300439X
    Filename: 
    - `CIV_MetaWRI_CanopyHeight_2020.tif`

14. **ETH Global Sentinel-2 10m Canopy Height (2020)**

     Available year 2020, the pixel has value and translated into below information: 

    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | canopy_height | meters | 0  | 50  | CanopyHeight_2000. |

     Data Reference: https://gee-community-catalog.org/projects/canopy/?h=canopy
     Reference: https://doi.org/10.48550/arXiv.2204.08322
     Filename: 
     - `CIV_ETH_CanopyHeight_2020.tif`

15. **Planet High resolution map of African tree cover**

    Available for year 2019, the pixel has percentage (%) value and translated into below information: 
    
    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | treecover | % | 0  | 100  | Tree canopy cover for year 2019. |

    Data Reference: https://gee-community-catalog.org/projects/af_trees/?h=canopy
    Reference: https://doi.org/10.1038/s41467-023-37880-4
    Filename: 
    - `CIV_Planet_TreeCover_2019.tif`

16. **MCD12Q1.061 MODIS Land Cover Type Yearly Global 500m**

    Available for year 2001-2023 as annual Land Cover and have various version below:
    
    | LC_Version | Description |
    |------------|-------------|
    | LC_Type1 | Annual International Geosphere-Biosphere Programme (IGBP) classification |
    | LC_Type2 | Annual University of Maryland (UMD) classification |
    | LC_Type3 | Annual Leaf Area Index (LAI) classification |
    | LC_Type4 | Annual BIOME-Biogeochemical Cycles (BGC) classification |
    | LC_Type5 | Annual Plant Functional Types (PFT) classification |
    | LC_Prop1 | FAO-Land Cover Classification System 1 (LCCS1) land cover layer |
    | LC_Prop2 | FAO-LCCS2 land use layer |
    | LC_Prop3 | FAO-LCCS3 surface hydrology layer |

    The pixel has value and translated into below information: 
    
    **LC_Type1**
    | Value | Description |
    |--------|-------------|
    | 1 | Evergreen Needleleaf Forests: dominated by evergreen conifer trees (canopy >2m). Tree cover >60% |
    | 2 | Evergreen Broadleaf Forests: dominated by evergreen broadleaf and palmate trees (canopy >2m). Tree cover >60% |
    | 3 | Deciduous Needleleaf Forests: dominated by deciduous needleleaf (larch) trees (canopy >2m). Tree cover >60% |
    | 4 | Deciduous Broadleaf Forests: dominated by deciduous broadleaf trees (canopy >2m). Tree cover >60% |
    | 5 | Mixed Forests: dominated by neither deciduous nor evergreen (40-60% of each) tree type (canopy >2m). Tree cover >60% |
    | 6 | Closed Shrublands: dominated by woody perennials (1-2m height) >60% cover |
    | 7 | Open Shrublands: dominated by woody perennials (1-2m height) 10-60% cover |
    | 8 | Woody Savannas: tree cover 30-60% (canopy >2m) |
    | 9 | Savannas: tree cover 10-30% (canopy >2m) |
    | 10 | Grasslands: dominated by herbaceous annuals (<2m) |
    | 11 | Permanent Wetlands: permanently inundated lands with 30-60% water cover and >10% vegetated cover |
    | 12 | Croplands: at least 60% of area is cultivated cropland |
    | 13 | Urban and Built-up Lands: at least 30% impervious surface area including building materials, asphalt and vehicles |
    | 14 | Cropland/Natural Vegetation Mosaics: mosaics of small-scale cultivation 40-60% with natural tree, shrub, or herbaceous vegetation |
    | 15 | Permanent Snow and Ice: at least 60% of area is covered by snow and ice for at least 10 months of the year |
    | 16 | Barren: at least 60% of area is non-vegetated barren (sand, rock, soil) areas with less than 10% vegetation |
    | 17 | Water Bodies: at least 60% of area is covered by permanent water bodies |

    **LC_Type2**
    | Value | Description |
    |--------|-------------|
    | 0 | Water Bodies: at least 60% of area is covered by permanent water bodies |
    | 1 | Evergreen Needleleaf Forests: dominated by evergreen conifer trees (canopy >2m). Tree cover >60% |
    | 2 | Evergreen Broadleaf Forests: dominated by evergreen broadleaf and palmate trees (canopy >2m). Tree cover >60% |
    | 3 | Deciduous Needleleaf Forests: dominated by deciduous needleleaf (larch) trees (canopy >2m). Tree cover >60% |
    | 4 | Deciduous Broadleaf Forests: dominated by deciduous broadleaf trees (canopy >2m). Tree cover >60% |
    | 5 | Mixed Forests: dominated by neither deciduous nor evergreen (40-60% of each) tree type (canopy >2m). Tree cover >60% |
    | 6 | Closed Shrublands: dominated by woody perennials (1-2m height) >60% cover |
    | 7 | Open Shrublands: dominated by woody perennials (1-2m height) 10-60% cover |
    | 8 | Woody Savannas: tree cover 30-60% (canopy >2m) |
    | 9 | Savannas: tree cover 10-30% (canopy >2m) |
    | 10 | Grasslands: dominated by herbaceous annuals (<2m) |
    | 11 | Permanent Wetlands: permanently inundated lands with 30-60% water cover and >10% vegetated cover |
    | 12 | Croplands: at least 60% of area is cultivated cropland |
    | 13 | Urban and Built-up Lands: at least 30% impervious surface area including building materials, asphalt and vehicles |
    | 14 | Cropland/Natural Vegetation Mosaics: mosaics of small-scale cultivation 40-60% with natural tree, shrub, or herbaceous vegetation |
    | 15 | Non-Vegetated Lands: at least 60% of area is non-vegetated barren (sand, rock, soil) or permanent snow and ice with less than 10% vegetation |

    **LC_Type3**
    | Value | Description |
    |--------|-------------|
    | 0 | Water Bodies: at least 60% of area is covered by permanent water bodies |
    | 1 | Grasslands: dominated by herbaceous annuals (<2m) including cereal croplands |
    | 2 | Shrublands: shrub (1-2m) cover >10% |
    | 3 | Broadleaf Croplands: bominated by herbaceous annuals (<2m) that are cultivated with broadleaf crops |
    | 4 | Savannas: between 10-60% tree cover (>2m) |
    | 5 | Evergreen Broadleaf Forests: dominated by evergreen broadleaf and palmate trees (canopy >2m). Tree cover >60% |
    | 6 | Deciduous Broadleaf Forests: dominated by deciduous broadleaf trees (canopy >2m). Tree cover >60% |
    | 7 | Evergreen Needleleaf Forests: dominated by evergreen conifer trees (canopy >2m). Tree cover >60% |
    | 8 | Deciduous Needleleaf Forests: dominated by deciduous needleleaf (larch) trees (canopy >2m). Tree cover >60% |
    | 9 | Non-Vegetated Lands: at least 60% of area is non-vegetated barren (sand, rock, soil) or permanent snow and ice with less than 10% vegetation |
    | 10 | Urban and Built-up Lands: at least 30% impervious surface area including building materials, asphalt and vehicles |

    **LC_Type4**
    | Value | Description |
    |--------|-------------|
    | 0 | Water Bodies: at least 60% of area is covered by permanent water bodies |
    | 1 | Evergreen Needleleaf Vegetation: dominated by evergreen conifer trees and shrubs (>1m). Woody vegetation cover >10% |
    | 2 | Evergreen Broadleaf Vegetation: dominated by evergreen broadleaf and palmate trees and shrubs (>1m). Woody vegetation cover >10% |
    | 3 | Deciduous Needleleaf Vegetation: dominated by deciduous needleleaf (larch) trees and shrubs (>1m). Woody vegetation cover >10% |
    | 4 | Deciduous Broadleaf Vegetation: dominated by deciduous broadleaf trees and shrubs (>1m). Woody vegetation cover >10% |
    | 5 | Annual Broadleaf Vegetation: dominated by herbaceous annuals (<2m). At least 60% cultivated broadleaf crops |
    | 6 | Annual Grass Vegetation: dominated by herbaceous annuals (<2m) including cereal croplands |
    | 7 | Non-Vegetated Lands: at least 60% of area is non-vegetated barren (sand, rock, soil) or permanent snow/ice with less than 10% vegetation |
    | 8 | Urban and Built-up Lands: at least 30% impervious surface area including building materials, asphalt, and vehicles |

    **LC_Type5**
    | Value | Description |
    |--------|-------------|
    | 0 | Water Bodies: at least 60% of area is covered by permanent water bodies |
    | 1 | Evergreen Needleleaf Trees: dominated by evergreen conifer trees (>2m). Tree cover >10% |
    | 2 | Evergreen Broadleaf Trees: dominated by evergreen broadleaf and palmate trees (>2m). Tree cover >10% |
    | 3 | Deciduous Needleleaf Trees: dominated by deciduous needleleaf (larch) trees (>2m). Tree cover >10% |
    | 4 | Deciduous Broadleaf Trees: dominated by deciduous broadleaf trees (>2m). Tree cover >10% |
    | 5 | Shrub: Shrub (1-2m) cover >10% |
    | 6 | Grass: dominated by herbaceous annuals (<2m) that are not cultivated |
    | 7 | Cereal Croplands: dominated by herbaceous annuals (<2m). At least 60% cultivated cereal crops |
    | 8 | Broadleaf Croplands: dominated by herbaceous annuals (<2m). At least 60% cultivated broadleaf crops |
    | 9 | Urban and Built-up Lands: at least 30% impervious surface area including building materials, asphalt, and vehicles |
    | 10 | Permanent Snow and Ice: at least 60% of area is covered by snow and ice for at least 10 months of the year |
    | 11 | Non-Vegetated Lands: at least 60% of area is non-vegetated barren (sand, rock, soil) with less than 10% vegetation |

    **LC_Prop1**
    | Value | Description |
    |--------|-------------|
    | 1 | Barren: at least of area 60% is non-vegetated barren (sand, rock, soil) or permanent snow/ice with less than 10% vegetation |
    | 2 | Permanent Snow and Ice: at least 60% of area is covered by snow and ice for at least 10 months of the year |
    | 3 | Water Bodies: at least 60% of area is covered by permanent water bodies |
    | 11 | Evergreen Needleleaf Forests: dominated by evergreen conifer trees (>2m). Tree cover >60% |
    | 12 | Evergreen Broadleaf Forests: dominated by evergreen broadleaf and palmate trees (>2m). Tree cover >60% |
    | 13 | Deciduous Needleleaf Forests: dominated by deciduous needleleaf (larch) trees (>2m). Tree cover >60% |
    | 14 | Deciduous Broadleaf Forests: dominated by deciduous broadleaf trees (>2m). Tree cover >60% |
    | 15 | Mixed Broadleaf/Needleleaf Forests: co-dominated (40-60%) by broadleaf deciduous and evergreen needleleaf tree (>2m) types. Tree cover >60% |
    | 16 | Mixed Broadleaf Evergreen/Deciduous Forests: co-dominated (40-60%) by broadleaf evergreen and deciduous tree (>2m) types. Tree cover >60% |
    | 21 | Open Forests: tree cover 30-60% (canopy >2m) |
    | 22 | Sparse Forests: tree cover 10-30% (canopy >2m) |
    | 31 | Dense Herbaceous: dominated by herbaceous annuals (<2m) at least 60% cover |
    | 32 | Sparse Herbaceous: dominated by herbaceous annuals (<2m) 10-60% cover |
    | 41 | Dense Shrublands: dominated by woody perennials (1-2m) >60% cover |
    | 42 | Shrubland/Grassland Mosaics: dominated by woody perennials (1-2m) 10-60% cover with dense herbaceous annual understory |
    | 43 | Sparse Shrublands: dominated by woody perennials (1-2m) 10-60% cover with minimal herbaceous understory |

    **LC_Prop2**
    | Value | Description |
    |--------|-------------|
    | 1 | Barren: at least of area 60% is non-vegetated barren (sand, rock, soil) or permanent snow/ice with less than 10% vegetation |
    | 2 | Permanent Snow and Ice: at least 60% of area is covered by snow and ice for at least 10 months of the year |
    | 3 | Water Bodies: at least 60% of area is covered by permanent water bodies |
    | 9 | Urban and Built-up Lands: at least 30% of area is made up ofimpervious surfaces including building materials, asphalt, and vehicles |
    | 10 | Dense Forests: tree cover >60% (canopy >2m) |
    | 20 | Open Forests: tree cover 10-60% (canopy >2m) |
    | 25 | Forest/Cropland Mosaics: mosaics of small-scale cultivation 40-60% with >10% natural tree cover |
    | 30 | Natural Herbaceous: dominated by herbaceous annuals (<2m). At least 10% cover |
    | 35 | Natural Herbaceous/Croplands Mosaics: mosaics of small-scale cultivation 40-60% with natural shrub or herbaceous vegetation |
    | 36 | Herbaceous Croplands: dominated by herbaceous annuals (<2m). At least 60% cover. Cultivated fraction >60% |
    | 40 | Shrublands: shrub cover >60% (1-2m) |

    **LC_Prop3**
    | Value | Description |
    |--------|-------------|
    | 1 | Barren: at least of area 60% is non-vegetated barren (sand, rock, soil) or permanent snow/ice with less than 10% vegetation |
    | 2 | Permanent Snow and Ice: at least 60% of area is covered by snow and ice for at least 10 months of the year |
    | 3 | Water Bodies: at least 60% of area is covered by permanent water bodies |
    | 10 | Dense Forests: tree cover >60% (canopy >2m) |
    | 20 | Open Forests: tree cover 10-60% (canopy >2m) |
    | 27 | Woody Wetlands: shrub and tree cover >10% (>1m). Permanently or seasonally inundated |
    | 30 | Grasslands: dominated by herbaceous annuals (<2m) >10% cover |
    | 40 | Shrublands: shrub cover >60% (1-2m) |
    | 50 | Herbaceous Wetlands: dominated by herbaceous annuals (<2m) >10% cover. Permanently or seasonally inundated |
    | 51 | Tundra: tree cover <10%. Snow-covered for at least 8 months of the year |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MCD12Q1
    Reference: https://doi.org/10.5067/MODIS/MCD12Q1.061
    Filename: 
    - `CIV_MCD12Q1_LandCover_{land_cover_version}_class_{yyyy}.tif` where `land_cover_version` = IGBP, UMD, LAI, BGC, PFT, LCCS1, LCCS2 and LCCS3. And `yyyy` = 2001-2023

17. **Landsat Collection 2 Tier 1 Level 2 Annual NDVI Composite**

    Available for year 1984-2024, the pixel has annual NDVI value and translated into below information: 
    
    | Name | Min | Max | Descriptions | 
    |----|----|----|----|
    | NDVI | -1  | 1  | Normalized Difference Vegetation Index. |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_COMPOSITES_C02_T1_L2_ANNUAL_NDVI
    Reference: 
    Filename: 
    - `CIV_Landsat_NDVI_{yyyy}.tif` where `yyyy` = 1984-2024

18. **MOD44B.006 Terra Vegetation Continuous Fields Yearly Global 250m**

    Available for year 2000-2020, the pixel has annual Vegetation Continuous Fields value and translated into below information: 
    
    | Name | Units | Min | Max | Descriptions | 
    |----|----|----|----|----|
    | Percent_pixel_tree_canopy | % | 0  | 100  | Percent of a pixel which is covered by tree canopy |
    | Percent_pixel_nontree_vegetation | % | 0  | 100  | Percent of a pixel which is covered by non-tree vegetation |
    | Percent_pixel_not_vegetated | % | 0  | 100  | Percent of a pixel which is not vegetated |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/MODIS_006_MOD44B
    Reference: https://doi.org/10.5067/MODIS/MOD44B.061
    Filename: 
    - `CIV_MOD44B_VCF{vcf_name}_{yyyy}.tif` where `vcf_name` = Percent_pixel_tree_canopy, Percent_pixel_nontree_vegetation and Percent_pixel_not_vegetated. And `yyyy` = 2000-2020

19. **MCD12Q2.006 Land Cover Dynamics Yearly Global 500m**

    Available for year 2001-2023, the pixel has annual Phenological Cycle value and translated into below information: 
    
    | Name | Min | Max | Scale | Units | Description |
    |------|-----|-----|-------|--------|-------------|
    | NumCycles | 0 | 7 | | Number | Total number of valid vegetation cycles with peak in product year |
    | Greenup1 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 first crossed 15% of the segment EVI2 amplitude - cycle 1 |
    | Greenup2 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 first crossed 15% of the segment EVI2 amplitude - cycle 2 |
    | MidGreenup1 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 first crossed 50% of the segment EVI2 amplitude - cycle 1 |
    | MidGreenup2 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 first crossed 50% of the segment EVI2 amplitude - cycle 2 |
    | Peak1 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 reached the segment maximum - cycle 1 |
    | Peak2 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 reached the segment maximum - cycle 2 |
    | Maturity1 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 first crossed 90% of the segment EVI2 amplitude - cycle 1 |
    | Maturity2 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 first crossed 90% of the segment EVI2 amplitude - cycle 2 |
    | MidGreendown1 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 last crossed 50% of the segment EVI2 amplitude - cycle 1 |
    | MidGreendown2 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 last crossed 50% of the segment EVI2 amplitude - cycle 2 |
    | Senescence1 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 last crossed 90% of the segment EVI2 amplitude - cycle 1 |
    | Senescence2 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 last crossed 90% of the segment EVI2 amplitude - cycle 2 |
    | Dormancy1 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 last crossed 15% of the segment EVI2 amplitude - cycle 1 |
    | Dormancy2 | 11138 | 32766 | | Days since 1 Jan 1970 | Date when EVI2 last crossed 15% of the segment EVI2 amplitude - cycle 2 |
    | EVIMinimum1 | 0 | 10000 | 0.0001 | | Segment minimum EVI2 value - cycle 1 |
    | EVIMinimum2 | 0 | 10000 | 0.0001 | | Segment minimum EVI2 value - cycle 2 |
    | EVIAmplitude1 | 0 | 10000 | 0.0001 | | Segment maximum - minimum EVI2 - cycle 1 |
    | EVIAmplitude2 | 0 | 10000 | 0.0001 | | Segment maximum - minimum EVI2 - cycle 2 |
    | EVIArea1 | 0 | 3700 | 0.1 | | Sum of daily interpolated EVI2 from Greenup to Dormancy - cycle 1 |
    | EVIArea2 | 0 | 3700 | 0.1 | | Sum of daily interpolated EVI2 from Greenup to Dormancy - cycle 2 |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/MODIS_006_MOD44B
    Reference: https://doi.org/10.5067/MODIS/MOD44B.061
    Filename: 
    - `CIV_MCD12Q2_LCD{description}_{yyyy}.tif` where `description` = NumCycles, Greenup1, Greenup2, MidGreenup1, MidGreenup2, Peak1, Peak2, Maturity1, Maturity2, MidGreendown1, MidGreendown2, Senescence1, Senescence2, Dormancy1, Dormancy2, EVIMinimum1, EVIMinimum2, EVIAmplitude1, EVIAmplitude2, EVIArea1, and EVIArea2. And `yyyy` = 2001-2023

20. **MCD15A3H.061 MODIS Leaf Area Index/FPAR 4-Day Global 500m**

    Available for year 2003-2023, the pixel has monthly  value and translated into below information: 
    
    | Name | Min | Max | ScaleFactor | Description |
    |------|-----|-----|-------|-------------|
    | Fpar | 0 | 100 | 0.01 | FPAR absorbed by the green elements of a vegetation canopy |
    | Lai | 0 | 100 | 0.1 | One-sided green leaf area per unit ground area in broadleaf canopies; one-half the total needle surface area per unit ground area in coniferous canopies |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MCD15A3H?hl=en
    Reference: https://doi.org/10.5067/MODIS/MCD15A3H.061
    Filename: 
    - `CIV_MCD15A3H_FPAR_{yyyy}.tif` where `yyyy` = 2003-2023
    - `CIV_MCD15A3H_LAI_{yyyy}.tif` where `yyyy` = 2003-2023

21. **MOD17A3HGF.061 and MYD17A3HGF.061: Terra and Aqua Net Primary Production Gap-Filled Yearly Global 500m**

    Available for year 2001-2023, the pixel has annual value and translated into below information: 
    
    | Name | Units | Min | Max | ScaleFactor | Description |
    |------|--------|-----|-----|-------------|-------------|
    | Gpp | kg*C/m^2 | 0 | 65500 | 0.0001 | Gross primary productivity |
    | Npp | kg*C/m^2 | -30000 | 32700 | 0.0001 | Net primary productivity |

    Data Reference: https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD17A3HGF and https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MYD17A3HGF
    Reference: https://doi.org/10.5067/MODIS/MOD17A3HGF.061 and https://doi.org/10.5067/MODIS/MYD17A3HGF.061
    Filename: 
    - `CIV_MXD17A3HGF_GrossPrimaryProductivity_{yyyy}.tif` where `yyyy` = 2003-2023
    - `CIV_MXD17A3HGF_NetPrimaryProductivity_{yyyy}.tif` where `yyyy` = 2003-2023

22. **JRC Tropical Moisture Forest, Annual Change 1990-2023**

    Available for year 1990-2023, the pixel has annual value and translated into below information: 
    
    | Value | Description |
    |--------|-------------|
    | 1 | UndisturbedTMF |
    | 2 | DegradedTMF |
    | 3 | DeforestedLand |
    | 4 | ForestRegrowth |
    | 5 | PermanentSeasonalWater |
    | 6 | OtherLandCover |

    Data Reference: https://forobs.jrc.ec.europa.eu/TMF/data
    Reference: https://doi.org/10.1126/sciadv.abe1603
    Filename: 
    - `CIV_TMF_AnnualChanges_{yyyy}.tif` where `yyyy` = 1990-2023

23. **Global 30m Land Cover Change Dataset (1985-2022) GLC_FCS30D**

    Available year 5-yearly (1985, 1990, 1995), Annual (2000-2022), the pixel has value and translated into below information: 
    
    | Value | Description |
    |--------|-------------|
    | 0 | Rainfed_cropland |
    | 1 | Herbaceous_cover_cropland |
    | 2 | Tree_or_shrub_cover_cropland |
    | 3 | Irrigated_cropland |
    | 4 | Open_evergreen_broadleaved_forest |
    | 5 | Closed_evergreen_broadleaved_forest |
    | 6 | Open_deciduous_broadleaved_forest |
    | 7 | Closed_deciduous_broadleaved_forest |
    | 8 | Open_evergreen_needle_leaved_forest |
    | 9 | Closed_evergreen_needle_leaved_forest |
    | 10 | Open_deciduous_needle_leaved_forest |
    | 11 | Closed_deciduous_needle_leaved_forest |
    | 12 | Open_mixed_leaf_forest |
    | 13 | Closed_mixed_leaf_forest |
    | 14 | Shrubland |
    | 15 | Evergreen_shrubland |
    | 16 | Deciduous_shrubland |
    | 17 | Grassland |
    | 18 | Lichens_and_mosses |
    | 19 | Sparse_vegetation |
    | 20 | Sparse_shrubland |
    | 21 | Sparse_herbaceous |
    | 22 | Swamp |
    | 23 | Marsh |
    | 24 | Flooded_flat |
    | 25 | Saline |
    | 26 | Mangrove |
    | 27 | Salt_marsh |
    | 28 | Tidal_flat |
    | 29 | Impervious_surfaces |
    | 30 | Bare_areas |
    | 31 | Consolidated_bare_areas |
    | 32 | Unconsolidated_bare_areas |
    | 33 | Water_body |
    | 34 | Permanent_ice_and_snow |
    | 35 | Filled_value |

    Data Reference: https://gee-community-catalog.org/tutorials/examples/glc_fcs30d_lulc/?h=cci#steps
    Reference: https://doi.org/10.5194/essd-16-1353-2024
    Filename: 
    - `CIV_GLC_FCS30D_{yyyy}.tif` where `yyyy` = 1985, 1990, 1995, 2000-2022.

