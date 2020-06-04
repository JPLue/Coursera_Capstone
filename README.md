# Coursera_Capstone

## 1. Problem Description and Background Discussion

### 1.1 Problem Description
As part of the Capstone Project for the Applied Data Science Coursera Course I have chosen to analyze the effectiveness of the Business Improvement Area (BIA) Program of Toronto, ON in Canada. The question I will answer is: **„Are most Venues located near or in Business Improvement Areas or are there clusters of Venues that should be made into BIAs?“** 

### 1.2 Background Discussion
The **Business Improvement Area (BIA)** is an association of commercial property owners and tenants within a defined area who work in partnership with the City to create thriving, competitive, and safe business areas that attract shoppers, diners, tourists, and new businesses. The question is how effective this association and the created Areas are for attracting shoppers, diners, tourists and new business. 

## 2. Data Description 

### 2.1 Description of Data and Data Source
The BIA layer represents the active BIAs in the City of Toronto that has been enacted by Council. Each BIA has been defined by a by-law and is represented by a Board of Management. The layer is updated as BIAs are created, amended or deleted by Council. This file is a polygon file that shows the BIAs Areas. The BIAs Data can be found at ...

Also I used Data about Boroughs and Neighborhoods in Toroto. They can be found as GeoJSON data at...

The second part of the data for the analysis comes via the Foursquare API. This dataset contains venues located in Toronto, there location, name, venue category and user rating. The information collected will be for all the central neighborhoods in Toronto.

### 2.2 How will the Data be used to solve the Problem
In a first step the location of the venues will be ploted on a map as overlay to the BIAs. This will show if most Venues are located within or very near BIAs and therefor give an answer about the effectivenes in promoting business near or in BIAs. 

In a second step the venues will be clustered depending on there location and the clusters plotted to show whether the clusters are located whitin the BIAs. This will help answer the second part of the question, whether there are clusters of venues in Toronto that are not part of any BIAs. This clusters could selected for the creation and localisation of future BIAs.

## 3. Problem solving

### 3.1 Getting the BIAs Data

Via the API provided by the City of Toronto 

### Loading Data about Neighborhoods

A GeoJSON file is loaded and the geometry and names of the Neighborhoods are used to plot them onto a map of Toronto.

### Plotting the BIA-Areas and Neighborhoods on a Map

The Folium library is use to plot the Neighborhoods and the BIA-Areas on the map of Toronto. The Map shows the shape of BIAs in green and the shape of Neighborhoods in blue.

### Getting Data for the Venues around the BIAs
Via a API request the data for the venues in all Neighborhoods are collected and stored in a Data Frame for easy data manipulation. The definied function will get, depending on the location (center of Neighborhood), the names of the venues within a radius of 800 m, the exact location of the venues (latitude, longitude) and the venue category. 

### Preparing the Venue Data for Analysis

Cleaning the Data, droping missing values, converting Latitude and Longitude to float.

## Analysing the Data
The collected Data is analised in the following ways:
1. Mapping the Venues with a Heatmap to visualise the Venue Density
2. Clustering the Venues with HDBSCAN
3. Mapping the Clusters to visualise Venue Clusters
4. Identify Clusters without BIAs

### 1. Mapping the Venue Density

Using a Heatmap it is possible to show the Density of Venues for the Neighborhoods. As we can see most Venues are located in or very near BIAs. This is a first and only visual indication for the effectivnes of BIAs to promote the creation of Venues.

### 2. Clustering the Locations with HDBSCAN

**HDBSCAN** is an algorithmen to find areas or clusters with a high desity of data points. The algorithm makes few assumptions about the shape of the cluster, instead it looks for areas with a higher density of data points than the surrounding areas. It is non-parametric method that looks for a cluster hierarchy shaped by the multivariate modes of the underlying distribution [https://towardsdatascience.com/understanding-hdbscan-and-density-based-clustering-121dbee1320e]. It excells therefor with data that has the following characteristics:
- Arbitrarily shaped clusters
- Clusters with different sizes and densities
- Noise
In this case HDBSCAN is used to find clusters of Venues based on there Location. The parameters for tuning the algorithmen allow to set the method for calculating the distance between the location points to **Haversine Distance**. Haversine Distance is a method for calculating distance between Geographical Points (Latitude, Longitude). The second parameter set is the **min_cluster_size** and the third **epsilon** a measure for the max distance between the points. Min_cluster_size is set to min 5 Venues per cluster and epsilon to max 200 meters. 

### 3. Visualising and Mapping the Clusters

To visualise the clustered Venues their center is calculated as the mean Latitude and Longitude for each cluster and then plotted onto the map of Toronto. As the map shows most clusters are located within the boundaries of the BIAs. The mapping also shows that some Venue clusters have not BIA nearby. This would be candidates for future development of BIAs. 

### 4. Identifing Clusters without BIA

To identify Clusters that are not near an existing BIA and could therefore be places for future BIAs the distance between each cluster and its nearest BIA is the key. For that the Haversine distance between each Cluster and each BIA is calculated and the BIA with the shortest distance for each Cluster is selected. The resulting data frame shows the distance for each pair of Cluster and BIA. If that distance is greater than the selected distance of 800 m the cluster is identified as not directly belonging to the BIA. 

As we can see **35 Clusters without an BIA nearby** exist. This would be prime locations for future development through BIAs. Of all Clusters of Venues in Central Toronto 70 are located directly on or very close to the center of BIAs. This shows how effective BIAs are in promoting Business nearby.