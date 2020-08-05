# PROJECT OBJECTIVE

The objective of this data cleaning project is to refine the publicly available dataset “US Farmer market” dataset available in the website to derive valuable business insights.

https://www.ams.usda.gov/local-food-directories/farmersmarkets

We used OpenRefine v3.3 to perform the initial cleaning of the raw dataset and then SQLite3 to perform integrity constraint checks and violation clean-ups on the dataset. The final cleaned dataset was further validated for special use-cases, and uploaded the original raw dataset and the final cleaned dataset in the Box folder at https://uofi.app.box.com/folder/119390153548 for review. The YesWorkflow model is used to annotate the various steps followed in the data cleaning process. We used the OpenRefine to YesWorkflow model toolkit (OR2YW v0.0.16) to render visual representations of workflows and provenance using Graph Viz.

## PART 1: OVERVIEW AND INITIAL ASSESSMENT ON THE DATASET

### 1.1	DATASET OVERVIEW

•	The US Farmers market dataset contains a total of 59 data elements.
•	FMID is a numeric field with a unique identifier to identify each Farmers market. There seems to be no duplicate values in FMID.
•	Market name is a text field containing the name of the farmer's market.
•	Website, Facebook, Twitter, YouTube, and Other Media
are all text fields containing either website or social media contacts such as twitter ID, channel name etc.,
•	Street, City, County, State and Zip contains address information about the farmer's market.
•	Season1Date, Season1Time, Season2Date, Season2Time, Season3Date, Season3Time, Season4Date, Season4Time contains various season date and time the respective Farmer's market are open during a year.
•	Columns X and Y looks like to represent the geographic latitude and longitude of the of the farmer's market
 
•	Column Location provides additional location details such as nearest landmark of the Farmer's market. This seems to be a text field.
•	Credit is a single character field with either Y or N denoting whether the payment via credit card is accepted or not. On initial assessment it seems all the records have this value populated in the dataset.
•	Columns WIC, WICcash, SFMNP, SNAP are all single character fields with either Y or N values denoting whether payment through food assistance programs is either accepted or not
•	A list of 30 different indicator fields with respect to the characteristics of various food products sold in the farmers market. On initial assessment all these columns look good with no discrepancies.
•	Updatetime denotes date and time when the data about farmer's market is captured

### 1.2	LIST OF DATA QUALITY ISSUES

•	Market name seems to contain lot of text variations for same Farmer market name.
•	Most of the values for the social media columns are blank
•	County and city names contain variations in text for the same value.
•	zip contains some invalid zip codes.
•	The date format for season dates seems to be not consistent across the records. Some records contain only month name, and some contains only from date etc.,
•	Most of the farmer's market have season1 and season 2 values and only very less records contain season three and four values.
•	Columns X and Y names are not clear, and we assume it represents the geographic location of the latitude and longitude of the farmers market.

### 1.3	IS THE DATA CLEAN ENOUGH FOR THE USE CASES?

Though dataset have the above noted quality issues, we could use the dataset for the below list of use cases:
•	To identify the list of Farmer's markets nearest based on the geographic location.
•	To identify the list of Farmer's markets which are open based on seasonality.
•	To identify list of Farmer's markets which accept credit card
•	To identify list of Farmer's markets which accept food coupons such as WIC, WICcash etc.,
•	To identify list of Famer's markets based on the various sold products such as seafood, pet food, vegetables etc.,

## PART 2: DATA CLEANING STEPS USING OpenRefine
Various technique which includes text Facets cluster and GREL are implemented for the various data elements in the Farmers market dataset using google OpenRefine tool and the dataset is refined

## PART 3: DEVELOPING A RELATIONAL SCHEMA
The cleaned dataset from openRefine is imported into sqllite database and various query operation are performed to check for integrity constraints such as duplicate FMID, duplicate FMID for the same market name and zip code combination etc., Finally a cleaned version of the dataset is obtained from this step.

## PART 4: CREATING A WORKFLOW MODEL
YesWorkflow model/ prototype has been used to annotate the data cleaning procedure followed, and to generate visual model representations using graphviz. The workflows have been created for both overall data cleaning process as well as the open refine tool specific operation history to represent the data provenance better.

![yesWorkFlow Model](https://github.com/bsathyamur/FarmersMarket_Data_Cleaning/blob/master/Overall_Workflow-img.png)

## PART 5: CONCLUSION:

The USDA Farmers Markets Dataset is overall a well-structured dataset except that data is incomplete and inconsistent on several columns which required many parts of data to be cleaned and reformatted to improve consistency and integrity. There are few columns in the dataset which becomes completely unusable such as “updateTime”, and “Season1StartDate”, which is very inconsistent and contains either “Month” or “DD/MM/YYYY” in variable text format, and there were overlapping seasonal dates observed, which makes it unusable specifically for use-cases related to seasonal date specific search queries. Lot of anomalies have been observed with respect to invalid date ranges with Season_To_Date being less than the From_Date and those have been cleaned up. With these cleaned up season1 start and end dates, we can search for farmers market for a specific seasonality more accurately.

Duplicate entries of Market information with different FMIDs but with similar Market Name profile information, address etc. have also been identified as Integrity violations through queries done through SQLite database. These potential duplicates have been cleaned up for more accurate representations.

Overall, the dataset was cleaned appropriately for the hypothetical use cases. Zip code data has also been validated for accuracy and the discrepancies were eliminated, thus making it usable for comparison between the number of farmers markets in each city or states and to use with geographic positioning related use cases

