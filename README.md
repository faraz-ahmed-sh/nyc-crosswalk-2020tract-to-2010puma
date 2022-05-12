# nyc-crosswalk-2020tract-to-2010puma

Code to create the 2020 tract to vintage 2010 PUMA's Crosswalk using NYC Open Data Portal and Geopandas.


[PUMA Boundaries](https://data.cityofnewyork.us/City-Government/Election-Districts/h2n3-98hq): Census-defined boundaries that roughly approximate the city's Community Districts. Each PUMA covers at least 100,000 residents.


[Census Tracts](https://data.cityofnewyork.us/City-Government/2010-Census-Tracts/fxpq-c8ku): The census tracts are small, permanent geographical unit of a county or its geographical equivalent. According to the Census Bureau, the number of people that reside in a census tract is between 1,200 and 8,000. Per Decennial 2010, there are 2,168 census tracts in NYC. 

Analyzing data for smaller geographies can allow for better analysis and census tracts tend to be great in that sense. However, their results should be interpreted with caution when using ACS estimates due to its small sample size.