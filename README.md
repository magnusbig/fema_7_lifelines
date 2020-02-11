# Utilizing Yelp data to estimate the number of businesses in a given locality and categorizing them according to FEMA's seven lifelines
## Summary:
Disaster could destroy your community’s infrastructure and damage homes and severely hurt the economy. Prior to and During disaster it is important to understand the projected and actual effect of the event on the community, including its effect on critical services a community needs to function.

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

Publish an interactive dashboard for FEMA that will provide, 

•	distribution of ‘community life lines’ in a given area.
 We will accomplish this by collecting data about all the businesses in a given area and categorizing them in to one of the life lines and dashboarding it.      
•	distribution of  ‘flood risk ’  in a given area

•	‘vulnerability index’ in a given area


## Strech Goal (ideas)
  * publish a API that will combine the  business category to mapping for any future project.
  
  * provide API that can be used by Yelp or FEMA to provide to business owner
    ** to be official partner of FEMA to earn one or more badges.
    ** update status of the business , (during disastor) which can be used by first responsders, FEMA to assess the situation
    <Please add here>
 
  ##. MVP high level Approach
  
     ###### Data collection  :
     
     1. Business data - Yelp/ Google places
     2. Flood risk data - TBD
     3. Social Vulnerability Index -  https://svi.cdc.gov/Documents/Data/2016_SVI_Data/SVI2016Documentation.pdf
     
      ###### Data wrangling  :
      1. Categorize business in to the FEMA life lines (to create a new column.)
      2. Model  risk as  "High" , "Medium" , "low" - for a given community.
      3. Build dashboard to display distribution of  3 peices of information (risk of flooding , SVI , lifeline).
      
      
    
  
