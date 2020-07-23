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
