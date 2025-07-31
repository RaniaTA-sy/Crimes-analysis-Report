# Crimes-analysis-Project

## table of contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)	
- [Data Cleaning Preparation](#data-cleaning-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results-Finding](#results-finding)
- [Recommendations](#recommendations)
## Project Overview
 This project aims to create a clear crime analysis dashboard that highlights crime patterns over time, by location, and by age group.
 It will help us understand when and where crimes are most common, identify the most dangerous times and groups, and provide insights to support better safety measures.
 Ultimately, it’s about turning data into actionable information to improve community safety.
 
 ![Download](https://github.com/RaniaTA-sy/Crimes-analysis-Report/blob/main/CrimeAnalysisReport.pdf)
 

### Data Sources

Crime Data: The primary dataset used in the analysis is a "crime_data.csv" file containing detailed information about all crimes recorded from 2021 to 2023.
### Tools 
- Power BI: Cleansing process by Power Query
  -[download here](http://micrsoft.com)
- Power BI: create an interactive Dashboard
### Data Cleaning Preparation
  We performed the following tasks: 
  1- data loading and inception
  2-Check the quality of the data and adjust the data type
  3- Data Modelling: Check the relationships between tables in the data model.
## Exploratory Data Analysis
EDA includes exploring the crime data to answer key questions such as:
Requirements
1.	Total crimes:
 sum of all reported crimes in the dataset
2.	Crimes distribution by year and yearly changes:
Analysis of crimes categorized by year, including insights into year over year changes.
3.	Monthly crime trend with percentage variance.
4.	Crimes by time range
Exploration of time occurrence within a definite time.
4. Hit map (show the crime distribution by weekdays and months.
5. Crimes by country.
6. Total resolved and unresolved crimes.
8. Identification of the most dangerous time of the day.
9. Figure out the most dangerous Age Group, which made the largest number of crimes.
10. Explain what the most common Crimes are in each country.
### Data Analysis
Include the functions and steps to figure out the previous questions
-	We have an Excel document, crimes_Data
-	Upload 2 tables (crimes_tbl, crimes_type) 
-	Crimes_tbl has 7 columns (Crime ID	/ Crime Date/Country/ Crime Type ID/ Resolved/ People Involved	/Crime Time)
-	Check the quality of the data and adjust the data type (change the data type of crime time to time only).
-	From the crime time column, add a column, a conditional column named Day_period, to add AM/PM period time.
-	 Check the relationship between tables by the data model.
-	In the people table, add a custom column called Age by using 
(Duration.Totaldays(#date(2025,1,1)- [Birth Date]) / 365.25
-	Add a conditional column called Age group to group people by Age.
Add Calendar Table as a date table by  DAX function 
``` DAX Function
(calander_table = CALENDAR(MIN(Crimes_tbl[Crime Date]),MAX(Crimes_tbl[Crime Date]))
```
Add a year column using a DAX function 
``` DAX Function
(Year = YEAR(calander_table[Date])
-Add Month_name column by Dax function ( FORMAT(calander_table[Date],"mmm")
-Add Month_number column by Dax function (FORMAT(calander_table[Date],"mm")
-Add Month_year column by Dax function (FORMAT(calander_table[Date],"MM-YYYY")
-Add Day_name Column by Dax function (FORMAT(calander_table[Date],"ddd")
- Add Day_number column by Dax function (WEEKDAY(calander_table[Date],2)
-Add IsweekEnd column by Dax function (IF(calander_table[Day_number]>5 ,"WeekEnd","weekDay")

-Add Quarter column by Dax function (Quarter = FORMAT(calander_table[Date],"\Q q")
- Mark the calendar table as a date table
- sort the month_number column by month_number column to arrange the months from Jun to dec.
-Add New Table for creating Measures called Measures by DAX function
``` DAX Function 
Measures {Blank ()}
-Add New Measure: Total_crimes = COUNTROWS(Crimes_tbl)
Create New Measure Named Tab(year) and include YOY change, previous crimes year, and add a positive/negative arrow by the Dax function 
Tab(Year) = 
VAR _prevYear = CALCULATE([Total_crimes],SAMEPERIODLASTYEAR(calander_table[Date])
)
VAR _YOYChange = 
IF(_prevYear<> BLANK(),
[Total_crimes] - _prevYear,BLANK()
)
VAR _positiveIcon = UNICHAR(9650)
VAR _NegativeIcon = UNICHAR(9660)
RETURN 
SWITCH(TRUE(),
_YOYChange =0 ,_YOYChange & "-",
_YOYChange >=1,_positiveIcon & _YOYChange,
_YOYChange <1,_NegativeIcon &_YOYChange
```
-Add a stacked column chart between total crimes and year.
``` DAX Function
-Add conditional formatting for YOYChange by adding a new measure
CF(Year) = 
VAR _prevYear = CALCULATE([Total_crimes],SAMEPERIODLASTYEAR(calander_table[Date])
)
VAR _YOYChange = 
IF(_prevYear<> BLANK(),
[Total_crimes] - _prevYear,BLANK()
)
VAR _positiveIcon = UNICHAR(9650)
VAR _NegativeIcon = UNICHAR(9660)
RETURN 
SWITCH(TRUE(),
_YOYChange =0 ,”Grey”
_YOYChange >=1,”Green”
_YOYChange <1,”Red”
```
-Add a line chart to find the number of crimes over the month
Create a new measure of crimes previous month 
``` DAX Function
CrimePrevMonth = CALCULATE([Total_crimes],DATEADD(calander_table[Date],-1,MONTH))
```
``` DAX Function
-Create New Measure called Tab(Month) and include MOM change
Tab(Month) = 
VAR _Pct_MOMchange =
DIVIDE([Total_crimes]-[CrimePrevMonth],[CrimePrevMonth])
VAR _positiveIcon = UNICHAR(9650)
VAR _negativeIcon = UNICHAR(9660)
VAR _NegativePct = -0.01
RETURN
   SWITCH(TRUE(),
_Pct_MOMchange = 0.0,"  ",
_Pct_MOMchange >=_NegativePct,"▲"& FORMAT(_Pct_MOMchange,"0%"),
_Pct_MOMchange< _NegativePct,"▼"&FORMAT(_Pct_MOMchange,"0%")
   )
```
``` DAX Function
-Create a new measure for applying conditional formatting
CF(Month) = 
VAR _Pct_MOMchange =
DIVIDE([Total_crimes]-[CrimePrevMonth],[CrimePrevMonth])
VAR _positiveIcon = UNICHAR(9650)
VAR _negativeIcon = UNICHAR(9660)
VAR _NegativePct = -0.01
RETURN
    SWITCH(TRUE(),
_Pct_MOMchange = 0.0,"Gray",
_Pct_MOMchange >_NegativePct,"Green",
_Pct_MOMchange< _NegativePct,"Red"
)
```
-Create a Hit Map By Adding a Matrix Table
-Create a Bar chart of total crimes and weekdays
-Create a stacked chart for total crimes and months.
-Create a Map to figure out which country has the highest number of crimes.

### Results-Finding

1-The percentage of total crimes increased by 184% in 2012, and decreased by 17.8% in 2023.
2- Months with high crime numbers are (March-May-July-September-October).
3-The most dangerous time is from 9 pm to 12 pm
4-The most dangerous Hour is 11:30:00 pm
5-Max crime type is violence and sexual.
6 - Weekdays with most crimes start with Sunday, then Wednesday, and Saturday.
### Recommendations
-Increase police watches in the evening on Sundays and Wednesdays, particularly between 11:30 PM and late at night, when crimes are most likely.
-Plan safety awareness campaigns during September and October to help communities stay safe during peak periods.
-Work closely in partnership with communities to bring an end to violence and sexual crimes, working primarily where these crimes happen most often.
-Special attention must be given to the increase in crime rates in 2022, and proactive steps to keep those numbers low in the future.











