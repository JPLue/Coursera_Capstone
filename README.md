# Coursera_Capstone

## 1. Problem Description and Background Discussion
### 1.1 Problem Description
As part of the Capstone Project for the Applied Data Science Coursera Course I have chosen to analyze the Business Improvement Area (BIA) Program of Toronto, ON in Canada. The question I will answer is: **„Are most Venues in Central Toronto located near or in Business Improvement Areas? A second question, but closely related to the first is: Are there clusters of Venues not located in BIAs that would therefore be favorable locations for future BIAs?“** 

Both questions are of interest to city planers, politicians and venue owners or business people planing the location of future venues. City planers and politicians will be interested in areas that could be favorable locations for future BIAs. And business people in the correlation between BIAs and the location of venues, since it enables a better planing of future venue locations.

### 1.2 Background Discussion
The **Business Improvement Area (BIA)** is an association of commercial property owners and tenants within a defined area who work in partnership with the City to create thriving, competitive, and safe business areas that attract shoppers, diners, tourists, and new businesses. Each BIA has been defined by a by-law and is represented by a Board of Management. The question is how effective this association and the created Areas are for attracting business in form of Venues. 

## 2. Data Description 
### 2.1 Description of Data and Data Source
To answer the question data from the following three sources is used:
- BIA Data: The BIA data represents the active BIAs in the City of Toronto that have so far been enacted by Council. The BIA layer is updated as BIAs are created, amended or deleted by Council. The BIA data frame includes information about 83 BIAs and data about their id, name, location, area and polygon shape. From this BIA data frame only the 74 BIAs located in the central area of Toronto are used. The BIAs Data can be found as GeoJSON file at [Toronto BIA Data](https://open.toronto.ca/dataset/business-improvement-areas/) and can be accessed via an API or downloaded as file.
- Neighborhood Data: Data about the Boroughs and Neighborhoods in Toroto. The Neighborhood data frame includes information about 140 Neighborhoods in Toronto and there id, name, location, area and polygon shape. From this 140 Neighborhoods only the 89 neighborhoods located in the central area of Toronto are used. This data can be found as GeoJSON data at [Toronto Neighborhood Data](https://open.toronto.ca/dataset/neighbourhoods/).
- Venue Data: Data about the Venues located within Toronto is collected via the Foursquare API. This dataset contains venues located in Toronto, there id, location, name and venue category. Venue data will be collected for each of the 79 Neighborhoods. The collected venue data includes information about 2694 venues. 

### 2.2 How will the Data be used to solve the Problem
In a first step the BIA and Neighborhood data will be reduced to include only BIAs and Neighborhoods located in the central area of Toronto. The resulting data will be plotted on a map of Toronto to show the areas of interest for this analysis, the location of the BIAs and the area, the Neighborhoods, for which data about Venues is collected. 

In a second step the density of venues will be plotted as a heatmap onto the map of Toronto. This will show the location of areas with a high density of venues and therefore give a first overview wether venues are mostly located near or in BIAs.

In a third step the venues will be clustered depending on there location and there proximity to each other. This will be done using the HDBSCAN machine learning algorithm. The location of each cluster will be plotted onto the map to show his location in relation to the BIAs. This will answer the first question wether most Venues are located in or very near BIAs.

In a fourth step the distance between the center of each cluster and the center of each BIA will be calculated. This will allow the matching of each cluster to his nearest BIA. Based on this distance clusters that are to far away from BIAs to be related to the BIA are designated as venue clusters that could locations for possible future BIAs. This will answer the question wether there are venue clusters that would could be favorable locations for future BIAs.

## 3. Methology used to analyse the data
The collected Data is analysed in the following ways:
1. Mapping the Venues with a Heatmap to visualise the Venue Density
2. Clustering the Venues with HDBSCAN
3. Mapping the Clusters to visualise Venue Clusters
4. Identify Clusters without BIAs

### 3.1 Mapping the Venue Density
Using a Heatmap it is possible to show the Density of Venues for the Neighborhoods. As we can see most Venues are located in or very near BIAs. This is a first and only visual indication for the effectivnes of BIAs to promote the creation of Venues.

![DensityMapVenues](Data/Density_map_venues.png)

### 3.2 Clustering the Locations with HDBSCAN
**HDBSCAN** is an algorithmen to find areas or clusters with a high desity of data points. The algorithm makes few assumptions about the shape of the cluster, instead it looks for areas with a higher density of data points than the surrounding areas. It is non-parametric method that looks for a cluster hierarchy shaped by the multivariate modes of the underlying distribution [https://towardsdatascience.com/understanding-hdbscan-and-density-based-clustering-121dbee1320e]. It excells therefor with data that has the following characteristics:
- Arbitrarily shaped clusters
- Clusters with different sizes and densities
- Noise
In this case HDBSCAN is used to find clusters of Venues based on there Location. The parameters for tuning the algorithmen allow to set the method for calculating the distance between the location points to **Haversine Distance**. Haversine Distance is a method for calculating distance between Geographical Points (Latitude, Longitude). The second parameter set is the **min_cluster_size** and the third **epsilon** a measure for the max distance between the points. Min_cluster_size is set to min 5 Venues per cluster and epsilon to max 200 meters. 

![DataFrameClusters](Data/Venues_clustered_based_on_location.png)

### 3. Visualising and Mapping the Clusters

To visualise the clustered Venues their center is calculated as the mean Latitude and Longitude for each cluster and then plotted onto the map of Toronto. As the map shows most clusters are located within the boundaries of the BIAs. The mapping also shows that some Venue clusters have not BIA nearby. This would be candidates for future development of BIAs. 

![ClusterBIARelations](Data/Relation_Cluster_BIA.png)

### 4. Identifing Clusters without BIA

To identify Clusters that are not near an existing BIA and could therefore be places for future BIAs the distance between each cluster and its nearest BIA is the key. For that the Haversine distance between each Cluster and each BIA is calculated and the BIA with the shortest distance for each Cluster is selected. The resulting data frame shows the distance for each pair of Cluster and BIA.

![DistanceMatrix](Data/Distance_matrix_Cluster_BIAs.png)

If the distance between Cluster and nearest BIA is greater than 800 m the cluster is identified as not directly belonging to the BIA. Based on this the data frame is split in clusteres with an BIA nearby (800 m) and clusters without BIA nearby. Both are shown in the following scatterplot.

![ScatterplotDistance](Data/Relation_Custer_BIA.png)

This data is mapped onto Toronto resulting in the following map. The map shows **35 Clusters without an BIA nearby**. This clusteres would be prime locations for future location of BIAs. Also the map shows all 70 clusters of venues in central Toronto 70 that are located directly on or closer than 800 m apart from an existing BIA. This shows how effective BIAs are in promoting Business nearby.

![MapClusterBIA](Data/Clusters_with_and_without_BIAs.png)