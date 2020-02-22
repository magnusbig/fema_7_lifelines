# Utilizing the number of businesses data in a given locality and categorizing them according to FEMA's seven lifelines
## Summary:
Disaster could destroy your community’s infrastructure and damage homes and severely hurt the economy. Prior to and during the disaster it is important to understand the projected and actual effect of the event on the community, including its effect on critical services a community needs to function.

FEMA (Federal emergency Management Agency) broadly categories these services under one of the seven “community lifelines”. Lifelines are the most fundamental services in the community that, when stabilized, enable all other aspects of society to function.


<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/lifelines.png" style="width: 500px">

The integrated network of assets, services, and capabilities that provide lifeline services are used day-to-day to support the recurring needs of the community and enable all other aspects of society to function.When disrupted, decisive intervention (e.g., rapid re-establishment or employment of contingency response solutions) is required to stabilize the incident.

One of the functions of FEMA is provide help to mitigate the loss of life and property by lessening the impact of disaster. For mitigation to be effective it is critical to understand the distribution, number of the lifelines in a given community and their status during disaster, so an appropriate response can be dispatched to stabilize the health of lifelines.

## Goal:
In order to provide a deeper solution, we plan to limit the scope to all areas in Colorado and study the effects of “flooding” on the several areas.Our team will like to build a tool that can assist FEMA in 2 main categories

### Before disaster:

Preparedness:  by providing a distribution of lifelines and risk score of flooding in a given area lastly provide some measure  of social vulnerability of the community to withstand a  disastor (suffering and financial loss ).
Above three. factor can be useful in assessing if current life line infrastructure is sufficient for a community and make specific recommendation to FEMA and community leaders identify gaps and build more robust life line infrastructure.


### During and after the Disaster:
Business owners can officially partner with FEMA and earn a lifeline badge they qualify for, and can display on the yelp profile, during disaster business owners can provide the status if they are open , closed, which can be displayed in real time to know evaluate the status of the lifeline on an interactive dashboard.


## MVP Goal:

1. Find and categorize FEMA lifelines in Denver 
2. Calculate flood risk for census tracts around Denver, using a   
Risk  =  Hazard  x  (Vulnerability  -  Resources)



## Stretch Goal (ideas)
  * publish a API that will combine the  business category to mapping for any future project.
  
  * provide API that can be used by Yelp or FEMA to provide to business owner
    ** to be official partner of FEMA to earn one or more badges.
    ** update status of the business , (during disastor) which can be used by first responsders, FEMA to assess the situation
    <Please add here>
       
     
 ## Data collection and cleaning
  
For the initial data gathering, we utilized the Google Place Nearby API using Denver, Colorado (Latitude: 39.7392, Longitude:  -104.9902) as the point of origin with a 50,000 meter radius. Once the data was captured, we removed all information except for geometry, name, types, and vicinity and removed any duplicate data. Using the geometry data, we extracted the latitude and longitude data and calculated the distance from point of origin for each business. Also we utilized the latitude and longitude data for each business to gather the elevatin data using the Google Elevation API.

As a group we assigned each type catagory to one of the seven lifelines. We then used this information to assign a lifeline number to each business based on its assigned type. A hierarchy logic was developed to prevent duplicates across multiple lifelines.

The clean data was then saved for the next phase.
  
  Output File Data Definition: 

  |Variable Name|Type|Decription|
|---|---|---|
|geometry|	object|	Latitude and Longitude and box geo information|
|latitude|	float|	Latitude info extracted from geometry data|
|longitude|	float|	Longitude info extracted from geometry data|
|distance|	float|	The distance from point of origin (km)|
|elevation|	int|	The elevation from sea level (m)|
|name|	object|	Business name|
|types|	object|	Type of business|
|vicinity|	object|	Business address|
|lifeline_number|	Int|	Number assignment for lifeline (1-7)|
|lifeline_category|	object|	Description of lifeline|

  ## EDA 
  ######  Community Life line frequency distribution and Elevations
   <img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/lifelines_frequency_distribution.png" style="width: 500px">
 
 ###### Life line  distribution by geography
  <img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/lifeline_geographic_distribution.png" style="width: 500px">
## Modelling 
  
### Create Zones
In order to properly get a resource score to calculate the comprehensive risk score an area needed to be determined to get the proper count of lifelines and population. <br>

Census tracts were considered for this but ultimately deemed to be too irregular in size and shape and many of the smaller tracts did not contain very many FEMA lifelines. Instead zones were created to cluster lifelines together based on proximity. With this approach, both a large sample of lifelines and population could be analyzed. <br>

Hospitals served as the center of these zones. They were selected because of their importance to a community and the limited amount of them for a given geographic region. Once the hospitals were identified they then were clustered. DBSCAN, an unsupervised learning model, was utilized to identify clusters of hospitals from one based on proximity of one another. Once these clusters were identified the average geographic coordinate was found among the cluster by averaging the latitude and longitude values of each hospital in each cluster. From this new center point a circular zone was created with a radius of approximately 6.9 miles or 14 miles in diameter. <br>

<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/zones_hospitals.png" style="width: 100px">
<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/zones_dbscan.png" style="width: 100px">
<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/zone_centers.png" style="width: 100px">
<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/zones_lifelines.png" style="width: 100px">



Once the zones had been defined the lifelines within them were counted and the population of the census tracts that fell within the zone were totaled. From here a zone resource score could be calculated. A weighted score was applied: <br>


| Weight | Lifeline             |
|--------|----------------------|
| 0.2    | Safety & Security    |
| 0.2    | Food, Water, Shelter |
| 0.3    | Health & Medical     |
| 0.1    | Energy               |
| 0.1    | Communication        |
| 0.1    | Transportation       |
| N/A    | Hazardous Materials  |


**Zone Lifeline Score** = Total Count of Resource * Lifeline Weight

**Zone Weighted Resource Score** = Total of all Zone Lifeline Scores

**Standard Score** = Zone Weighted Resource Score * (1 / Max Zone Weighted Resource Score)

Once this Zone Resource score was established it then was standardized. Essentially it was converted to a 0 to 1 scale were the highest score was equal to 1, this new score is the Standard Score. The Standard Score could then be assigned to each census tract. The final Resource Score was then calculated by dividing the Standard Score by the population of the Census tract.

**Resource Score** = Standard Score / Census Population

This resource score is a proper representation of the resources available to every person in their respective census tract. It is used in the overall risk calculation as the Resource variable.


**Risk = Hazard (Vulnerability - Resource)**

To evaluate the expression in between the parenthesis the CDC’s Social Vulnerability Index (SVI) was used in place of Vulnerability. The issue that arose here is that in the instance that the Resource score is larger than the Vulnerability score, the expression will result in a negative number. This does not make logical sense because a census tract should not have a negative risk score. <br>

To resolve this issue the evaluation was set to have the limit of 0.01. Essentially if the census tract was well equipped with resources and had a low Vulnerability rating at most they could be assessed to have a 0.01 risk score. This eliminated the possibility of a negative risk score and also made the statement that each census tract is subject to some amount of risk.

From here the Hazard score could be added to the equation to provide an overall Risk score of each census tract. 
  
 **Vulnerability Score**

We used the CDC's [Social Vulnerability Index](https://svi.cdc.gov/factsheet.html) percentile ranking as our vulnerability score. This score is calculated for each census tract and takes into account socioeconomic, household composition, Race/Ethnicity/Languages and Transportation for a given area to assess their vulnerability in the event of a disaster. The percentile ranking ranks each census tract in the US from 0% to 100% (most vulnerable).  

<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/den_vulnerability.png" style="width: 100px">

**Flood Hazard Score**

*Step 1 Gather Elevation*: We first took 20 random points within each census tract and called the Google Maps Elevation API to get the elevation at various points within each tract. We then assigned each tract the minimum elevation value from that set. We chose minimum because in looking at floods we care most about low points.

*Step 2 Find Elevation Difference to Nearby Tracts*: We found all the tracts within a 3km radius of the centroid of each tract and compared the minimum of the tract to the minimum elevation of the nearby tracts. By doing this we were able to find which tracts included the lowest point in the surrounding area, the premise being that flood risk is higher if one is at the lowest surrounding point such that water will flow there. One flaw that immediately became apparent here is, as one moves out of densely populated areas the tracts are large enough that our 3km radius does not capture any other tracts so all those area appear to be a local low point. We were unable to figure out a solution for this during the current timeline but are happy with the performance of our method within dense, urban areas.

*Step 3 Scale Elevation Differences and Calculate Score*: Our final step consisted of first scaling our elevation differences to a 0-1 scale and then converting those values to a hazard score from 1-10. We scaled the data because we tried various standardization methods and found that our elevation data wasn't accurate enough to provide a perfect mapping of flood risk but the scaled version gave us a pretty close representation of FEMA's flood zone map for Denver, at least downtown. The 1-10 scale for hazard was chosen because it is easily understandable. Below you can see the comparison of our flood hazard rating with the FEMA map for Denver. You'll note that while metric is far from perfect it does capture the main flood zone through Denver quite well, which made us feel that this is at the least a good starting point for taking about flood hazard.

<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/den_flood_risk.png" style="width: 100px">
<img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/zfema_flood_hazard.png" style="width: 100px">

**Risk = Hazard (Vulnerability - Resource)**

To evaluate the expression in between the parenthesis the CDC’s Social Vulnerability Index (SVI) was used in place of Vulnerability. The issue that arose here is that in the instance that the Resource score is larger than the Vulnerability score, the expression will result in a negative number. This does not make logical sense because a census tract should not have a negative risk score. <br>

To resolve this issue the evaluation was set to have the limit of 0.01. Essentially if the census tract was well equipped with resources and had a low Vulnerability rating at most they could be assessed to have a 0.01 risk score. This eliminated the possibility of a negative risk score and also made the statement that each census tract is subject to some amount of risk.

From here the Hazard score could be added to the equation to provide an overall Risk score of each census tract. 
  
  
 ## Comprehensive Risk Dashboard
  
  Following interactive dashboard built in Tableau to visualize the individual components of equation which was used as heuristic to calculate the 'comprehensive risk' score, The bottom dashboard gives a visual indication of risk associated with
different census tracts for the Denver region. The score ranges from 0-10 with with 0 being the lowest risk to 10 being the hiest risk.

  <img src="https://github.com/magnusbig/fema_7_lifelines/blob/master/images/comprehensive_risk.png" style="width: 500px">
  
  
  ## Further Improvement Section
  1. The 'hazard index' was derived using elevation of randomly selected locations in a census tract , This could further be refined by taking other factors in to account like the rainfall and absorptive capacity of soil , flow capacity of rivers
urban drainage basin quality.
  2. Much of the code utilizes GeoPandas which is not particularly fast, in order to be used in more geographies and provide a quick view of risk the code needs to be optimized.
  3. Using API's to collect data from the CDC directly instead of downloading the data.
  4. Consider more directly which lifelines might be affected in the event of a flood and how that effects a communities capability to respond.
