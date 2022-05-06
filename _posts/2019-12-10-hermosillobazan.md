---
title: "Team hermosillobazan (Cross-Dataset Analysis)"
date: 2019-12-10
tags: [Data Science]
excerpt: "Data analytics custom website. Utilizing Bootstrap, API calls and postgreSQL to construct data tables of different statistics in the NBA."
---
## Introduction
This project started mid September 2019 as part of a semester project for CS 327E
Elements of Databases at The University of Texas at Austin. We formed in teams of
two tasked with browsing through publicly available datasets and picking two do cross-data
tables and analysis.

For our main dataset my partner and I chose the national storm events dataset by
the National Oceanic and Atmospheric Administration (NOAA), found [here](https://public.enigma.com/browse/collection/national-climatic-weather-center-storm-events/3d0d8bdf-b885-48b9-a3b1-8a8441fc0131).
And we chose to cross it with the National Fire Incident Reporting System
of the U.S Government's Federal Emergency Management Agency, found [here](https://public.enigma.com/browse/collection/nfirs-wild-land-incidents/952d1599-65cc-4374-afa8-b07f6cdb2f98). Our goal was to see how
storm events and wildfires compare when considering the time of year, location, and
damage caused.

The github repository could be found at: [github](https://github.com/Basilio0505/hermosillobazan-CrossDatasetProject)

## Tools
To start off our project my partner and I utilized public dataset website, [Enigma.com](https://enigma.com/).
After retrieving our data we utilized Google Cloud Platform tools to clean, organize,
combine, and analyze our chosen datasets.

Using Google Cloud Storage we stored our datasets and future data tables, using BigQuery
we broke up and organized with MySQL. Finally with the Google Could Dataflow tool
we constructed multiple new data tables with Jupyter Notebooks and with the AI Platform
we sped up the creation of new tables out of the thousands of records we had in our two
datasets.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/hermosillobazan/AirflowDAG.jpg" alt="AirflowDAG">

Here is an example of the tables constructed:
```sql
-- Creates a table of events and where and when they happened as well as interesting facts such as damage in crops, deaths and injuries they caused.
CREATE TABLE storm_events_modeled.StormEvents AS
SELECT *
FROM
(SELECT
event_id, event_type, year, month_name, begin_day, state_fips, cz_fips, injuries_direct, injuries_indirect, deaths_direct, deaths_indirect, damage_crops
FROM storm_events_staging.StormEvents_2013
UNION DISTINCT
SELECT event_id, event_type, year, month_name, begin_day, state_fips, cz_fips, injuries_direct, injuries_indirect, deaths_direct, deaths_indirect, damage_crops
FROM storm_events_staging.StormEvents_2012
UNION DISTINCT
SELECT event_id, event_type, year, month_name, begin_day, state_fips, cz_fips, injuries_direct, injuries_indirect, deaths_direct, deaths_indirect, damage_crops
FROM storm_events_staging.StormEvents_2011
UNION DISTINCT
SELECT event_id, event_type, year, month_name, begin_day, state_fips, cz_fips, injuries_direct, injuries_indirect, deaths_direct, deaths_indirect, damage_crops
FROM storm_events_staging.StormEvents_2010)
ORDER BY event_id, event_type, year, month_name, begin_day, state_fips, cz_fips, injuries_direct, injuries_indirect, deaths_direct, deaths_indirect, damage_crops

--Create Date as an entity and create a primary key.

CREATE TABLE storm_events_modeled.Date AS
SELECT *
FROM
(SELECT distinct (safe_cast ( begin_date_time = DATE)) as date, year, month_name, begin_day
FROM storm_events_staging.StormEvents_2013
UNION DISTINCT
SELECT distinct (safe_cast ( begin_date_time = DATE)) as date, year, month_name, begin_day
FROM storm_events_staging.StormEvents_2012
UNION DISTINCT
SELECT distinct (safe_cast ( begin_date_time = DATE)) as date, year, month_name, begin_day
FROM storm_events_staging.StormEvents_2011
UNION DISTINCT
SELECT distinct (safe_cast ( begin_date_time = DATE)) as date, year, month_name, begin_day
FROM storm_events_staging.StormEvents_2010)
ORDER BY date



--Creates a table with locations as primary keys and can be mapped to county name given state fips and county fips.

CREATE TABLE storm_events_modeled.Locations AS
SELECT *
FROM
(SELECT  distinct( begin_lat_begin_lon_appended)  as location,  cz_name, state_fips, cz_fips
FROM storm_events_staging.StormEvents_2013
WHERE begin_lat_begin_lon_appended is not null
UNION DISTINCT
SELECT distinct( begin_lat_begin_lon_appended)  as location,  cz_name, state_fips, cz_fips
FROM storm_events_staging.StormEvents_2012
WHERE begin_lat_begin_lon_appended is not null
UNION DISTINCT
SELECT distinct( begin_lat_begin_lon_appended)  as location,  cz_name, state_fips, cz_fips
FROM storm_events_staging.StormEvents_2011
WHERE begin_lat_begin_lon_appended is not null
UNION DISTINCT
SELECT distinct( begin_lat_begin_lon_appended) as location,  cz_name, state_fips, cz_fips
FROM storm_events_staging.StormEvents_2010
WHERE begin_lat_begin_lon_appended is not null)
ORDER BY location,cz_name, state_fips, cz_fips



--Creates a table with all unique state FIPS code along with the name of the state or territory.
--The state_fips being the primary key
CREATE TABLE storm_events_modeled.State AS
SELECT *
FROM
(SELECT DISTINCT state_fips, state
FROM storm_events_staging.StormEvents_2013
WHERE state_fips IS NOT null OR state IS NOT null
UNION DISTINCT
SELECT DISTINCT state_fips, state
FROM storm_events_staging.StormEvents_2012
WHERE state_fips IS NOT null OR state IS NOT null
UNION DISTINCT
SELECT DISTINCT state_fips, state
FROM storm_events_staging.StormEvents_2011
WHERE state_fips IS NOT null OR state IS NOT null
UNION DISTINCT
SELECT DISTINCT state_fips, state
FROM storm_events_staging.StormEvents_2010
WHERE state_fips IS NOT null OR state IS NOT null)
ORDER BY state_fips           
                 
```
## Obstacles
With this project specifically designed to untilize two seperate datasets curated by seperate organizations,
their were surely obstacles in our path that presented itself. To formulate a core objective and questions 
to answer between the datasets we had to keep in mind there would be missing data entires, values that would
not match up between datasets and size differences.

For example, the Wild Land Incidents dataset merely provided dates and not seperated values for month, day, 
nad year as the Storm Events Dataset did. As well as, the Wild Land Incidents dataset only state abbreviations
rather than full state names. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/hermosillobazan/BeamPipelines.jpg" alt="Beam">

We were able to utilize Apache Beam to format the data to allow us to create primary keys in which we can use 
to compare data.For example below is the formatting we used to alter the data records state value:
```python
# DoFn to perform on each element in the input PCollection.
class ExpandStateFn(beam.DoFn):
    def process(self, element):
        state_id, notneeded = element 
        
        if state_id == "AL":
            statename = "ALABAMA"
            state_fips = 1
        elif state_id == "AK":
            statename = "ALASKA"
            state_fips = 2 #ALL OTHER US STATES
		else:
            statename = "OTHER"
            state_fips = 0

        return [(state_id, statename, state_fips)]  

# PTransform: format for BQ sink
class MakeRecordFn(beam.DoFn):
    def process(self, element):
        state_id, statename, state_fips = element
        record = {'state_id':state_id, 
               'statename':statename, 
               'state_fips':state_fips}
        return [record] 
```

## Results
With our best efforts to wokr through the obstabcles presented in front of us we were able to answer the quesions
we presented at the start of the project. Although, surely there is plenty of outliers and missing data that had
affected our results. We did see some correlations between the two datasets.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/hermosillobazan/Graphs.jpg" alt="Graphs">