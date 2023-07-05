# DataAnalystCase

Cocoa farmers supply chain data. 

---------------------------------------------
# [Data Analyst Case ReadME]

---------------------------------------------
## Summary

Registration and Mapping datasets on cocoa farmer's supply chain. The Registration dataset includes farmer information and the Mapping dataset includes plot information. 

Included are: data exports, requested KPI's, inconsistencies, and documentation.


---------------------------------------------
## Excel tabs


#### 
- [Overview]: Includes KPI's & links to related worksheets
- [Ghana Registered]: Registration database, original table provided
- [Ghana Mapping]: Mapping database, original table

- [Q_Ghana Registered]: Query built from Registration table
- [Q_Ghana Mapping]: Query built from Mapping table
-[Lookup]: Original Lookup Table

-[Ghana Registered - No Code]: Table of Registrants missing farmer_society Code
-[Ghana Mapping-No Code]: Table of Farmers missing farmer_society Code
- [FullOuterJoin]: All of Mapping dataset, all of registered dataset, and all matches between the two
- [FarmersNotOnMapping]: Copy of the Query and sheet 'Q_LeftAntiOnRegistered'. Lists all Farmers that are on the Registration dataset, but not on Mapping
- [FarmersNotOnRegistration]: Copy of the Query and sheet 'Q_LeftAntiOnMapping'. Lists all the Farmers that are on the Mapping dataset, but not Registration.
-[Q_LeftAntiOnRegistered]: Query building the 'FarmersNotOnMapping' table 
-[Q_LeftAntiOnMapping]: Query building the 'FarmersNotOnRegitsration' table


---------------------------------------------
## Materials and Methods

- [Excel, data transformation with Power Query].


---------------------------------------------
## Additional Notes

Documentation:

1.) First, I opened the provided Excel file of the supplier's most recent data export, which is entitled 'WEEK 3 REGISTRATION AND MAPPING DATABASE'.
2.) I selected the contents of the 'Ghana Registered' sheet and inserted a table. I then repeated this step on 'Ghana Mapping'. Creating tables enables Power Query editor to query both tables and therefore identify any requested KPIs. 
3.) Within Cell J2 of Sheet 2 entitled 'Ghana Registered', I created a VLOOKUP in the Registered table, checking each row's farmer_society against the farmer_society provided in the Lookup table. This pulled in the unique Code for each farmer_society. I then repeated this step in cell H2 of the Mapping table.
4.) On both the Registered and Mapping tables, I concatenated the [farmer_id] + [farmer code] together to make a unique ID. In 'Ghana Registered this was column K. In the 'Ghana Mapping' table, this CONCAT function was in column I. 
5.) While on the 'Ghana Registered' sheet I navigated to the Data ribbon, selected 'From Table/Range', creating a query from the selected table.  
6.) ERROR HANDLING/INCONSISTENCIES: The Query had one row of errors, which could be examined within Power Query Editor.I copied this row and pasted it into a separate sheet and entitled it 'Ghana Registered - No Code', since the farmer_society in this row could not be found in the Lookup table. I then located this row in the table, removed it from the Registered table, and ran the query again so it would finish without errors. 
7.) After identifying the errors in the Registered table, I navigated to the 'Ghana Mapping' table and on the Data ribbon selected 'From Table/Range' to query the Mapping information. 
8.) ERROR HANDLING / INCONSISTENCIES: The Query had errors due to about 900 farmer_society rows not having a match within the Lookup table. I copied these errors into a separate sheet entitled 'Ghana Regisered-No Code'. I then selected the Code column, and removed all blanks within that column to subsequently remove the 7 rows of errors. After the errors were saved in a separate sheet, I reran the query so it would finish without errors. 
9.) KPI's: 
a). To identify the unique number of farmers listed in the Mapping and Registration datasets, I entered a COUNTIF function for the farmer_id column on the 'Ghana Registered' table. This equation can be found on the Overview sheet. 
b). To find the Number of Farms, I entered a COUNTIF function for the farmer_society_Code column on the 'Ghana Mapping' table. This equation can be found on the Overview table. 
10.) To find the remaining KPI's, I queried the Mapping and Registered tables and joined them on the unique ID created from[farmer_society] + farmer_id]. This was a Full Outer Join, to see all Mapping Data, all Registered data, and all matches between the two tables. 
11.) KPI's continued: 
a.) To find the average number of farmers per farm, I first had to find the unique count of farms on the FullOuterJoin table. To find this count, I entered a COUNTIF function for the farmer Code (column H of the Query entitled 'FullOuterJoin'). In the table, I identified the total number of registered farmers with a farm matched to them. I did this by using a COUNTIF function where the farmer_id was present and therefore >= 1. This equation can be found in the overview page. The total number of rows of matched farmer_id data was 2253. Dividing this number by 77 total farms gives us an average of 29 farmers per farm. 
b.) To find the unique number of farms, I entered a COUNTIF function for the farmer_society_Code column on the 'Ghana Mapping' table. This equation can be found on the Overview table. 
c.) I then divided the total number of registrants by the unique count of farms. This value gives us the average number of farmers per farm.
12.) To list all farmers appearing on the Mapping dataset, but not on Registration, I queried the Mapping and Registration tables together again and joined them using a LeftAntiJoin with Mapping on the left. This table is saved as 'FarmersNotOnRegistration'.
13.) To list all farmers appearing on the Registration dataset but not on the Mapping dataset, I queried the Mapping and Registration tables together and joined thme using a LeftAntiJoin, this time with Registration on the left. This table is saved as 'FarmersNotOnMapping'.
14.) Calculations for KPI's were calculated on the Overview page. 


---------------------------------------------
## Questions/Commentary

Overall:

This was a great exercise for manipulating raw data while also working with a many to many relationship. It challenged the way I approached the data, for many reasons. One reason being that there were multiple files provided, but only one of the files was the full dataset and therefore was the only file which needed to be manipulated. 

Challenges:

When initially reading the instructions, I wanted to perform the VLOOKUP in the lookup table, instead of in the Registration and Mapping tables. The VLOOKUP when entered in the Lookup table only provided the first result or match, and did not provide all matches. I tried using INDEX() and MATCH() functions, and then realized I would be manually building the tables instead of letting Power Query do the work for me if I put the LOOKUP() function in the other tables. Error handling in Excel's Power Query Editor is slightly different than Power Bi's Power Query Editor, since errors would still appear even after removing or replacing them. Since I trusted Excel's ability to remove blanks for a specific column, I used that feature to remove the errors instead, however I normally would try manipulating the rows whenever possible and replacing the blanks. In this assignment, it may be helpful to have the errors separate from the dataset since it's possible the farmer_society field was BLANK by mistake. 

Improvements:

The Registration data could potentially be improved with the addition of a farmer name. It is clear that a farmer can work at multiple farms, but if a plot size were erroneously recorded twice and reporting two conflicting plot sizes, it may be difficult to verify which record holds the correctly reported shape_area. The entry_date and farmer_gender both help provide more context, but adding a farmer name could provide another layer of verification. 

Questions: 

1.) If the value 2 is recorded in the farm_plots table on the Registered table, does that count as 1 farm or 2 farms? I assumed it counted as 1 farm. 

2.) Does a farmer_society name represent a single farm? I assumed yes, and that the unique count of the farmer_society field represented the total count of farms. When researching some of the farmer_society names, it appeared many of the names matched to regions in Ghana and specifically Western Ghana. Since some farmer_society names appeared to be have the name of regions and since the column name had the word 'society' in it, it could be assumed that the farmer_society is not one farm but a collection of farms. If farmer_society is to represent a single farm, it could be helpful to have another identifying piece of information specificaly regarding the farm name. 

