---
layout: page
title: RP- Vulnerability modeling for sub-Saharan Africa
---


**Replication of**
# Vulnerability modeling for sub-Saharan Africa

Original study *by* Malcomb, D. W., E. A. Weaver, and A. R. Krakowka. 2014. Vulnerability modeling for sub-Saharan Africa: An operationalized approach in Malawi. *Applied Geography* 48:17–30. DOI:[10.1016/j.apgeog.2014.01.004](https://doi.org/10.1016/j.apgeog.2014.01.004)

Replication Authors:
Nick Nonnenmacher, Joseph Holler, Kufre Udoh, Open Source GIScience students of fall 2019 and Spring 2021

Replication Materials Available at: [nicknonnen/RP-Malcomb](https://github.com/nicknonnen/RP-Malcomb)

Created: `14 April 2021`
Revised: `27 April 2021`

## Acknowledgements

Thank you Kufre Udoh and Professor Joe Holler of Middlebury College for providing the
- Joe, Kufre, members of my group (Maja - esp for going over physical data together, Emma, Drew, Alitzel, Jacob)
- mention specifics, highlight at the front of my report

## Abstract

The original study is a multi-criteria analysis of vulnerability to Climate Change in Malawi, and is one of the earliest sub-national geographic models of climate change vulnerability for an African country. The study aims to be replicable, and had 40 citations in Google Scholar as of April 8, 2021.

This report is a reproduction of Malcomb et al.'s results, and aims to ....

## Original Study Information

The study region is the country of Malawi. The spatial support of input data includes DHS survey points, Traditional Authority boundaries, and raster grids of flood risk (0.833 degree resolution) and drought exposure (0.416 degree resolution).

The original study was published without data or code, but has detailed narrative description of the methodology. The methods used are feasible for undergraduate students to implement following completion of one introductory GIS course. The study states that its data is available for replication in 23 African countries.


### Data Description and Variables

###*Access and Assets Data*
Demographic and Health Survey data are a product of the United States Agency for International Development (USAID). Variables contained in this dataset are used to represent adaptive capacity (access + assets) in the Malcomb et al.’s (2014) study. These data come from survey questionnaires with large sample sizes.
The DHS data used in our study were collected in 2010. In Malawi, the provenance of the DHA data dates back as far as 1992, but has not been collected consistently every year. Each point in the household dataset represents a cluster of households with each cluster corresponding to some form of census enumeration units, such as villages in rural areas or city blocks in urban areas [DHS GPS Manual](/data/metadata/DHS_GPS_Manual_English_A4_24May2013_DHSM9.pdf). This means that each household in each cluster has the same GPS data. This data is collected by trained [USAID](https://www.usaid.gov/) staff using GPS receivers.
Missing data is a common occurrence in this dataset as a result of negligence or incorrect naming. However, according to the [DHS GPS Manual](/data/metadata/DHS_GPS_Manual_English_A4_24May2013_DHSM9.pdf), these issues are easily rectified and typically sites for which data does not exist are recollected. Sometimes, however, missing information is coded in as such or assigned a proxy location.
The DHS website acknowledges the high potential for inconsistent or incomplete data in such broad and expansive survey sets. Missing survey data (responses) are never estimated or made up; they are instead coded as a special response indicating the absence of data. As well, there are clear policies in place to ensure the data’s accuracy. More information about data validity can be found on the [DHS’s Data Quality and Use site](https://www.dhsprogram.com/data/Data-Quality-and-Use.cfm).
In this analysis, we use the variables listed in **Table 1** to determine the average adaptive capacity of each TA area. Data transformations are outlined below.

**Table 1:** DHS Variables used in Analysis

| Variable Code | Definition |
| ------------- | ------------- |
| HHID | "Case Identification" |
| HV001 | "Cluster number" |
|HV002 | Household number |
| HV246A |"Cattle own" |
|HV246D | "Goats own"|
|HV246E | "Sheep own" |
|HV246G | "Pigs own" |
| HV248 |"Number of sick people 18-59"|
| HV245 | "Hectares for agricultural land"|
|HV271 | "Wealth index factor score (5 decimals)"|
|HV251 | "Number of orphans and vulnerable children"|
|HV207 | “Has Radio” |
| HV243A | “Has a Mobile Telephone”|
|HV219 | Sex of Head of Household”|
|HV226 | “Type of Cooking Fuel” |
| HV206 |"Has electricty” |
|HV204 |“Time to get to Water Source”|

**Variable Transformations**

1. Eliminate households with null and/or missing values
2. Join TA and LHZ ID data to the DHS clusters
3. Eliminate NA values for livestock
4. Sum counts of all different kinds of livestock into a single variable
5. Apply weights to normalized indicator variables to get scores for each category (assets, access)
6. find the stats of the capacity of each TA (min, max, mean, sd)
7. Join ta_capacity to TA based on ta_id
8. Prepare breaks for mapping
9. Class intervals based on capacity_2010 field
10. Take the values and round them to 2 decimal places
11. Put data in 4 classes based on break values

###*Livelihood Zones Data*
add a short blurb here describing this Data

**Table 2:** Constructing Livelihood Sensitivity Categories

| Livelihood Sensitivity Category (LSC)  | Percent Contributing  | How LSC was constructed  |
| ------------- | ------------- | ------------- |
| Percent of food from own farm  |  6%  | Sources of food: crops + livestock  |
| Percent of income from wage labor  | 6%  | Sources of cash: labour etc. / total * 100  |
| Percent of income from cash crops  | 4%  | sources of cash (Crops): (tobacco + sugar + tea + coffee) + / total sources of cash * 100  |
| Disaster coping strategy  | 4%  | Self-employment & small business and trade: (firewood + sale of wild food + grass + mats + charcoal) / total sources of cash * 100  |

**Variable Transformations**

1. Join with DHS clusters to apply LHZ FNID variables
2. Clip TA boundaries to Malawi (st_buffer of LHZ to .01 m)
3. Create ecological areas: LHZ boundaries intersected with TA boundaries to clip out park/conservation boundaries and rename those park areas with the park information from TA data), combined with lake data to remove environmental areas from the analysis

###*Physical Exposure Data: Floods and Droughts*
add a short blurb here on each dataset


### Analytical Specification

The original study was conducted using ArcGIS and STATA, but does not state which versions of these software were used.
The replication study will use R 1.4.1106 and QGIS LTR 3.16.4-Hannover.

## Materials and Procedure

ADAPTIVE CAPACITY WORKFLOW [ASSETS & ACCESS]
*Process Adaptive Capacity*

1. Bring in DHS Data [Households Level] (vector)
2. Bring in TA (Traditional Authority boundaries) and LHZ (livelihood zones) data
3. Get rid of unsuitable households (eliminate NULL and/or missing values)
4. Join TA and LHZ ID data to the DHS clusters
5. Pre-process the livestock data Filter for NA livestock data Update livestock data (summing different kinds)
6. FIELD CALCULATOR: Normalize each indicator variable and rescale from 1-5 (real numbers) based on percent rank
7. FIELD CALCULATOR / ADD FIELD: Apply weights to normalized indicator variables to get scores for each category (assets, access)
8. SUMMARIZE/AGGREGATE: find the stats of the capacity of each TA (min, max, mean, sd)
9. Join ta_capacity to TA based on ta_id (Multiply by 20--meaningless??) I have a question about this (so do I) ln.216
10. Prepare breaks for mapping Class intervals based on capacity_2010 field Take the values and round them to 2 decimal places Put data in 4 classes based on break values
11. Save the adaptive capacity scores

```
- Bring in DHS Data [Households Level] (vector)
- FIELD CALCULATOR: Normalize each indicator variable into quintiles (0 is lowest, 5 is highest--we understand this doesn’t make sense if there are only 5 categories, but this is what the authors said they did)
- FIELD CALCULATOR / ADD FIELD: Apply weights to normalized indicator variables to get scores for each category (assets, access, livelihood sensitivity, physical exposure)
- FIELD CALCULATOR / ADD FIELD: Combine assets and access into adaptive capacity
- AGGREGATE: Aggregate into villages
- AGGREGATE: Aggregate into TA geometries, calculate average adaptive capacity score (Assets + Access) for each TA

**Results: Figs. 3 & 4 (for us, only most recent data will be used - equivalent to Fig. 4)**
```

HOUSEHOLD RESILIENCE & RASTER WORKFLOW [FINAL DELIVERABLE]
*Process Livelihood Results*

1. Load in LHZ geometries into R
2. Join LHZ sensitivity data into R code
3. Read in processed LHZ dataset
4. Join the data to the LHZ geometries
5. Put LHZ data into quintiles
6. Calculate capacity score based on values in Malcomb et al. (2014)

```
- RASTERIZE: turn household resilience at TA level into raster data at pixel size (30m? 90m?) of FEWSNET and UNEP
- Bring in FEWSNET data (raster) and UNEP/GRID data (raster)
- RASTER CALCULATOR: quintile (assign scores 0-5) and weight FEWSNET & UNEP/GRID data
- RASTER CALCULATOR: Using FEWSNET, UNEP/GRID, and rasterized DHS resilience data; Calculate household resilience using the following formula:
- Household Resilience = Adaptive Capacity + Livelihood Sensitivity - Physical Exposure
```

*Process Physical Exposure*

1. Load in UNEP rasterSet CRS for drought
2. Set CRS for flood
3. Clean and reproject rasters
4. Create a bounding box at extent of Malawi Where does this info come from
5. For Drought: use bilinear to avg continuous population exposure values
6. For Flood: use nearest neighbor to preserve integer values
7. CLIP the traditional authorities with the LHZs to cut out the lake
8. RASTERIZE the ta_capacity data with pixel data corresponding to capacity_2010 field
9. RASTERIZE the livelihood sensitivity score with pixel data corresponding to capacity_2010 field


*Raster Calculations*

1. Create a mask
2. Reclassify the flood layer (quintiles, currently binary)
3. Reclassify the drought values (quantile [from 0 - 1 in intervals of 0.2 =5])
4. AGGREGATE: Create final vulnerability layer using environmental vulnerability score and ta_capacity.

Finally, we georeferenced Figures 4 and 5 from Malcomb et al. (2014) in QGIS in order to compare the original study results to those produced by the above R script. This comparison was quantitatively demonstrated through a Spearman's Rho correlation test, presented here in ***Table X.***

## Replication Results

For each output from the original study (mainly figure 4 and figure 5), present separately the results of the replication attempt.

2.	State whether the original study was or was not supported by the replication
3.	State whether any hypothesis linked to a planned deviation from the original study was supported. Provide key statistics and related reasoning.

Figures to Include:
- map of resilience by traditional authority in 2010, analagous to figure 4 of the original study
- map of vulnerability in Malawi, analagous to figure 5 of the original study
- map of difference between your figure 4 and the original figure 4
- map of difference between your figure 5 and the original figure 5


![Fig4resilience](/assets/Fig4rep.png)
![resilience_comparison](/assets/Fig4comp.png)
![Fig5vulnerability](/assets/Fig5rep.png)
![vulnerability_comparison](/assets/Fig5comp.png)

## Unplanned Deviations from the Protocol

Summarize changes and uncertainties between
- your interpretation and plan for the workflow based on reading the paper
- your final workflow after accessing the data and code and completing the code

## Discussion

Provide a summary and interpretation of the key findings of the replication *vis-a-vis* the original study results. If the attempt was a failure, discuss possible causes of the failure. In this replication, any failure is probably due to practical causes, which may include:
- lack of data
- lack of code
- lack of details in the original analysis
- uncertainties due to manner in which data has been used

## Conclusion

Restate the key findings and discuss their broader societal implications or contributions to theory.
Do the research findings suggest a need for any future research?

## References

Include any referenced studies or materials in the [AAG Style of author-date referencing](https://www.tandf.co.uk//journals/authors/style/reference/tf_USChicagoB.pdf).

####  Report Template References & License

This template was developed by Peter Kedron and Joseph Holler with funding support from HEGS-2049837. This template is an adaptation of the ReScience Article Template Developed by N.P Rougier, released under a GPL version 3 license and available here: https://github.com/ReScience/template. Copyright © Nicolas Rougier and coauthors. It also draws inspiration from the pre-registration protocol of the Open Science Framework and the replication studies of Camerer et al. (2016, 2018). See https://osf.io/pfdyw/ and https://osf.io/bzm54/

Camerer, C. F., A. Dreber, E. Forsell, T.-H. Ho, J. Huber, M. Johannesson, M. Kirchler, J. Almenberg, A. Altmejd, T. Chan, E. Heikensten, F. Holzmeister, T. Imai, S. Isaksson, G. Nave, T. Pfeiffer, M. Razen, and H. Wu. 2016. Evaluating replicability of laboratory experiments in economics. Science 351 (6280):1433–1436. https://www.sciencemag.org/lookup/doi/10.1126/science.aaf0918.

Camerer, C. F., A. Dreber, F. Holzmeister, T.-H. Ho, J. Huber, M. Johannesson, M. Kirchler, G. Nave, B. A. Nosek, T. Pfeiffer, A. Altmejd, N. Buttrick, T. Chan, Y. Chen, E. Forsell, A. Gampa, E. Heikensten, L. Hummer, T. Imai, S. Isaksson, D. Manfredi, J. Rose, E.-J. Wagenmakers, and H. Wu. 2018. Evaluating the replicability of social science experiments in Nature and Science between 2010 and 2015. Nature Human Behaviour 2 (9):637–644. http://www.nature.com/articles/s41562-018-0399-z.
