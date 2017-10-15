--- 
layout: post 
title: MTA Data Analysis 
---  

The first project at Metis focused on using Python and Pandas to analyze multiple data sets. I worked with [Eric](https://github.com/ericchan24) and [Ming](https://github.com/tangming2008). We acted as consultants for a fictional non-profit organization, WomenTechWomenYes (WTWY). The organization has a summer gala at the beginning of every summer, and have asked us to help out. WTWY wanted us to use MTA subway data to help optimize the placement of their street teams in order to gather the most signatures, ideally from people who will attend the gala and donate. 

## Strategy

### Understand the Client Problem

In order to maximize converting signatures to becoming donors, we wanted to focus on two variables: targeting subway station entrances with the most foot traffic and targeting those most likely to contribute to WTWY's cause if they were attend the gala. Foot traffic is a data point that can be measured, but how could we define those most likely to donate? We thought about different demographic data and decided that housing value would be a good way to measure this likelihood. The zipcode of each station can be cross-referenced with Zillow's Home Value Index data. Zillow takes the median home value in a zipcode to get the estimate. More info about Zillow can be found [here](https://www.zillow.com/wikipages/What's-the-Zillow-Home-Value-Index/).  

### Make Assumptions and Clean the Data

The MTA data that we explored recorded the number of entries and exits at each turnstile for every station. We decided to focus on entries only since it would be easier for street teams to only worry about people entering the subway. WTWY holds their gala in the summer, so we analyzed data for May 2017. After defining our assumptions, we moved towards cleaning the MTA data. Pandas is great for finding errors or unusual trends in data that might not be so obvious. In the MTA data set, there were three main issues:
  * Not all turnstiles record entries on the same time interval 
  * Turnstiles will sometimes count backwards 
  * Turnstiles will reset their counter by incrementing the count to an unrealistically high number.

Once cleaned, the data can now be plotted and analyzed to help us come up with our recommendations.

### Analyze the Data and Provide Clear Recommendations

![Monthly Entries per Station](https://zachheick.github.io/images/monthly-entries-per-station.png){: .center-image }

Clean data allows us to group by the linename and station name to get the total entries for that station in May 2017. We wanted to break this down further and find which days of the week are busieset.

![Entries by day per Station](https://zachheick.github.io/images/entries-by-day-per-station.png){: .center-image }

The x-axis is each day in May 2017 and the y-axis is the total entries at a station on that day. The high and wide peaks show that weekdays are much busier than the low and narrow values, the weekend. This makes sense because most people work Monday through Friday and need to take the subway.

![House Values and Entries per Station](https://zachheick.github.io/images/housing-value-and-entries-per-station.png){: .center-image }

In order to maximize potential donations, we used housing value as a measurement of wealth. We took the busiest stations' zipcodes and matched them with the approximate Zillow estimate.  

We recommend targeting stations that both have a high amount of entries as well as being located in an area with a high housing value. Based on this criteria, we recommend the 14th Street - Union Square station, 42 Street - Port Authority station. We also recommend Grand Central Terminal. It is not in an area with a high housing value but has an overwhelming amount of MTA rides.

### Present findings

We presented our findings to the class and instructors as if they were the client. 

## Future work 

Given extended time to improve this project, we would consider using more variables other than just housing values. Some demographics that we could explore are areas with greater ratios of women to men, areas where there are more tech companies and universities, and salary by zipcode. All of these demographics could help improve our recommendations.

## What We Learned

### Analyzing Data as a Human

Oftentimes, it is easy to analyse data sets and come up with recommendations without thinking about the data from a human perspective. A good question that came up after our presentation was, "Do wealthy people even take the subway". A question like this did not cross our minds at all! Maybe some do, maybe some don't. If we decide that wealthy people are actually less likely to ride the subway, who else could we target? What other data sets can we use? Is there a cut-off for how much money you have vs. whether or not you take the subway? It is important to keep asking yourself questions about the recommendations received, and to keep making changes and explore other scenarios that can be solved with data.   

### Importance of how Data is Presented

Storytelling is a necessary skill to have as a data scientist. When presenting findings and recommendations to an audience, how you present yourself can be as important as the recommendations themselves. As a group we did an okay job of practicing our presentation, but we could have been more engaging with our audience.  

## Project source
The source can be found on my [github](https://github.com/ZachHeick/Project_Benson).

