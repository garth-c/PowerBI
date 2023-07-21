# PowerBI demo

Project purpose:
Demonstrate the basic functionality in PowerBI to process data, create data models, reports, data visualizations, and dashboards. 

Data description:
This data was a denormalized super set of data that I found on Kaggle.com. I separated the transaction data (details data) and put it into a separate file. I then created several master file tables (fact data) and put them into separate files. All of the fact tables and the details table have keys in order to demo the star schema approach to data model building. Also, all of these files are Excel files and I converted all of the relevant data to a table format in order to import them into PowerBI. 

For the fact table, I inserted an order number that is used as a key to easily connect the respective cost data for that specific order from a summarized order cost table. I also removed all of the extended amounts around sales totals and discounts in order to demonstrate the calculation functionality in DAX. I also rounded the units sold to an integer value along with the order unit sales price to keep the math simple. The details table dimensions are 700x8 and the fact tables are all very small with only a up to 6 or 7 records each and a few columns - depending on the table.

Overall, I normalized a denormalized data set to demonstrate how this would work in reality as well as I modified several key aspects of the source data to demonstrate multiple features within PowerBI. 

The file structure that I developed for this demo is shown below:

<img width="190" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/4da7ac15-ac6b-4ef1-948b-188c4e71d29a">

A snippet of the details table is below:

<img width="656" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/be6cff1c-c057-4335-a4da-701fb0750372">

A snippet of a typical fact table layout is below:

<img width="155" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/cfe2ed42-8f0a-465b-95ae-4dd1a77344be">

Note that no other data prep or data transformations were needed for these 6 files for this project.

---------------------------------------------------------------------------------------

# road map for this project
+ read in source data files with Power Query & the M language
+ create the data model
+ create the table calculations with DAX
+ create a report and a dashboard
+ interpret the results

----------------------------------------------------------------------------------------

# read in the source data files with Power Query and the M language

The details table import is shown below. All of the data types are what they need to be and no data transformation will be needed/
<img width="719" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/2818cdc3-8716-43c5-a548-68a0ae367943">

to make the import process a little more dynamic, I created a parameter that has the path on my computer in it and I inserted that in the Source step in Power Query 'Source' step. The parameter is called 'local_path'

<img width="202" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/b7832760-faeb-446d-a45a-eec1be8caeda">

The parameter is referenced in the 'Source' step below:

<img width="383" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/9d8ee9d5-d4cf-4424-ba89-5ae096b73b07">

A screen print of the M language used for this import is below. This is where changes could be made directly to the query instructions if the UI from Power Query would not meet the needs of the import.

<img width="698" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/cd0fcc68-fd02-4d06-9861-b783ae5d4ecd">

The same Power Query steps were followed for all of the tables imported into PowerBI

<img width="140" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/1a123dc3-d3f4-495d-963b-f6eb569eb5c7">

After all 6 tables were imported, the data model now needs to be developed.

------------------------------------------------------------------------------------------

# create the data model

The data model that I developed is shown below. This model uses the star schema approach where the transaction or details table is in the center and all of the related data from the master files or fact tables are then attached to it like points on a star. 

<img width="398" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/3927339d-2d79-4443-ad7a-fd2f7717aa1a">

A generic graphic for a star schema is below:
![image](https://github.com/garth-c/PowerBI/assets/138831938/e322e6a5-2ffe-4e1d-947f-138b808936cd)

If there were sub tables for the fact tables, then a snow flake schema would have been needed. 


The relationship between these tables is shown in the connectors and for the fact tables this is a many to 1 relationship. Below is a depiction of a many to 1 relationship for one of the fact tables to the detail tables. Since the details table will have many instances of the key value and the fact table will have only one instance of the key value, this is the many to 1 relationship that I am referring to. The only relatinship in this data model that has a different association is the order cost summary table to the details table. Since the cost data is presumed to be from a different table altogether, a summarization of the aggregated costs relative to the specific order number is the relationship. So each processed order will have a summarize cost to associated with it.

<img width="343" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/2e9512aa-7e9b-43a3-962d-41b904c9d571">

A screen shot of the imported data is shown below for reference.

<img width="476" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/f6d4e5ed-5623-4f11-b7eb-66710908329a">

----------------------------------------------------------------------------------------------------

# create the table calculations with DAX


--------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------

WORK IN PROGRESS BELOW:














