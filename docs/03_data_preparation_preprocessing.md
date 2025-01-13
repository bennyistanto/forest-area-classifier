# 3 Data Preparation & Preprocessing

Data preprocessing is a crucial step to ensure that all input layers share the same spatial reference system, resolution, and geographical extent. Here, we use **Google Earth Engine (GEE)** to streamline tasks such as clipping data to the country boundary, reprojecting to a chosen CRS, and resampling to a 100m scale. The resulting outputs can then be exported locally (e.g., to Google Drive) in GeoTIFF format for subsequent analysis in Python or other environments.

Below is an overview of the main procedures.

## 3.1 Harmonizing Spatial and Temporal Resolution

1. **Selecting the Study Area**  
   - We used a feature collection of country boundaries (`USDOS/LSIB_SIMPLE/2017`) and filtered it by a specific country code. Then we extracted the geometry to define our area of interest.

   ```js
   var selectedCountry = ee.FeatureCollection("USDOS/LSIB_SIMPLE/2017")
       .filter(ee.Filter.eq('country_co', selectedFips));
   var countryGeometry = selectedCountry.geometry();
   ```

2. **Reprojecting at 100m**  
   - Each dataset (ImageCollection or single Image) was reprojected (`reproject`) to a chosen EPSG (in this case `'EPSG:3857'`), with a scale of 100 meters. This ensures consistent spatial resolution across datasets.

   ```js
   var countryForest = forestCover.reproject({
       crs: 'EPSG:3857',
       scale: 100
   });
   ```

3. **Time-Specific Filtering (If Applicable)**  
   - For time-series datasets, we iterated over specific years (e.g., 2017, 2018, etc.) or year ranges, filtering ImageCollections by date. This way, each exported image reflects the coverage for that specific time or year.

   ```js
   var forestCover = ee.ImageCollection('JAXA/ALOS/PALSAR/YEARLY/FNF4')
                     .filterDate(year + '-01-01', year + '-12-31')
                     .mosaic();
   ```

## 3.2 Merging & Masking

1. **Clipping to Boundary**  
   - To avoid unnecessary data outside our study area, we clipped each image to the country geometry before export.

   ```js
   var clippedImage = originalImage.clip(countryGeometry);
   ```

2. **Mosaicking Multiple Scenes**  
   - If a dataset was available as a collection of multiple tiles (e.g., monthly or annual images), we often used `.mosaic()` to combine them. This step helps produce a single continuous coverage for the year or period of interest.

   ```js
   var forestCover = ee.ImageCollection('NASA/MEASURES/GFCC/TC/v3')
       .filterDate(year + '-01-01', year + '-12-31')
       .select('tree_canopy_cover')
       .mosaic();
   ```

3. **Optional Masking**  
   - Certain datasets (e.g., forest loss year) require masking of pixels not relevant to our classification or substituting invalid values with a reference image. We used `.mask()` or `.unmask()` where necessary to ensure valid pixel coverage.

   ```js
   var forestCoverFilled = forestCover.unmask(reference2000).mask(ee.Image(1));
   ```

4. **Exporting to Drive**  
   - Finally, each processed image is exported to Google Drive in GeoTIFF format, using consistent naming conventions (`.toDrive(...)`). We specified:
     - `description` to label the export
     - `folder` to organize files in Drive
     - `fileNamePrefix` to unify naming patterns
     - `region` for the bounding geometry
     - `scale` = 100
     - `crs` = `'EPSG:3857'`
     - `maxPixels` = `1e13` to allow for large exports

   ```js
   Export.image.toDrive({
     image: reprojectedImage,
     description: 'CIV_SomeDataset_2020',
     folder: 'CIV',
     fileNamePrefix: 'CIV_SomeDataset_2020',
     region: countryGeometry,
     scale: 100,
     maxPixels: 1e13,
     crs: 'EPSG:3857'
   });
   ```

## 3.3 Additional Ancillary Features

Beyond the standard layers (forest cover, canopy height, etc.), it may be valuable to include contextual features such as:
- **Distance to Roads**: Derived from OpenStreetMap or other sources to measure fragmentation or accessibility.  
- **Distance to Rivers**: Proximity to water sources can influence forest type and vegetation health.  
- **Climate Data**: Temperature and precipitation gradients may help differentiate moist forests from dry forests.  
- **Soil and Geology**: Certain soil types or geological formations correlate with specific forest ecosystems.

In practice, these ancillary data can also be ingested into GEE, clipped, and resampled in the same manner as the core datasets—ensuring everything remains aligned for pixel-wise analysis.

---

With all these preparations completed in GEE, we can now download a consistent, country-wide stack of GeoTIFF layers—at the same resolution, in the same coordinate system. This uniformity greatly simplifies subsequent data ingestion and analysis in Python, where we’ll merge features and run machine learning (e.g., Random Forest) to produce annual forest maps for MRV and other applications.