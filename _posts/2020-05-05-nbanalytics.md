---
title: "NBAnalytics.me"
date: 2020-05-05
tags: [Website, Bootstrap, HTML/CSS, Python, MySQL, postgreSQL, Data Science, Databases, API, Google Cloud Platform, 2020, Flask]
excerpt: "Data analytics custom website. Utilizing Bootstrap, API calls and postgreSQL to construct data tables of different statistics in the NBA."
---
## Introduction
This project started mid February 2020 as part of a semester project for CS 331E
Elements of Software Engineering II at The University of Texas at Austin. We formed
a team of six students and discussed what we wanted to accomplish, ultimately settling
on a shared love for sports leading to our NBA analytics website.

The website was to be hosted on Google Cloud Platform and was set to be completed
by the end of the semester, in early May. We used Agile development methodology to
prioritize tasks of importance with user stories. To accomplish our goals within
the time we used multiple common practice tools.

Here is a link to our website, however it may be down due to rising cost to host
on GCP. ([NBAnalytics](http://nbanalytics.me/)) Otherwise here is a link to the
GitLab repository:[Gitlab](https://gitlab.com/Basilio0505/cs331e-group-project)

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/nbanalytics/NBAnalytics_Home.jpg" alt="NBAnalytics Home Page">

Here is a walkthrouh of the website in case the link to visit the site is down.
<iframe width="560" height="315" src="https://www.youtube.com/embed/_yge5TOVouo" frameborder="0" allowfullscreen></iframe>

## Tools
For our website our team decided to use the cloud hosting service on Google Cloud
Platform, running it on the App Engine. For locally testing we used the web development
tool Flask. Our sole frontend tool we used is Bootstrap in order to create a simple
yet attractive design to our website.

<img src="{{ site.url }}{{site.baseurl}}/assets/images/nbanalytics/NBAnalytics_Teams.jpg" alt="NBAnalytics Teams Page">

Being an analytics website we gathered data from API calls from [balldontlie.io](https://www.balldontlie.io/#introduction)
and sourced player portrait images from [NBA.com](https://www.nba.com/players). We
organized this data into separate tables using database management tool postgreSQL.

<img src="{{ site.url }}{{site.baseurl}}/assets/images/nbanalytics/NBAnalytics_Games.jpg" alt="NBAnalytics Games Page">

Additionally, we constructed unittests to test any corner cases within possible data.
At the end of the semester we also ran an alternate version of our site on Docker
as part of an end of year presentation about the tool.

Here are some more screenshots of the site:
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/nbanalytics/NBAnalytics_Celtics.jpg" alt="NBAnalytics Celtics Page">

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/nbanalytics/NBAnalytics_Lebron.jpg" alt="NBAnalytics Lebron Page">

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/nbanalytics/NBAnalytics_About.jpg" alt="NBAnalytics About Page">
