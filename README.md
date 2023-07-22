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

### Go back to my profile page
[garth-c profile page] (https://github.com/garth-c)

---------------------------------------------------------------------------------------------

# road map for this project
+ read in source data files with Power Query & the M language
+ create the data model
+ create the table calculations with DAX
+ create multiple dashboards
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

There are 4 calculated fields that need to be determined using DAX. Note that there are other ways of doing this too such as with a measure, etc. but I wanted to showcase how DAX works. 

The first calculated field is to extend the sales total. This is the unit cost * the unit price. The DAX code to this is below.

```
extend_sales = CALCULATE(SUM('transaction_table'[unit price]) * SUM(transaction_table[units sold]))
```

<img width="347" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/e4884e4d-b436-4b9b-b316-94ebbf633dae">

The next calculated field to calculate the discount total. This is applying the discount rate (as a percent of the extended sales amount) from the discount rate fact table from the data model to the extendes sales amount that I just calculated above.

```
discount_amt = CALCULATE(SUM(transaction_table[extend_sales]) * SUM(discount_master[disc_prcnt]))
```

<img width="355" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/6d562ea1-927e-40fa-9b3b-b52a824532df">

The next calculated field is to determine the net sales amount which is the extended sales from above less the calculated discount total also calculated above.
```
net_sales = CALCULATE(SUM(transaction_table[extend_sales]) - SUM(transaction_table[discount_amt]))
```

<img width="344" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/a92ed285-5175-4afd-b56e-372397adb39b">

The last calculated field is to determine the gross margin for each sale. This is the net sales from above less the aggregated total order costs from the order cost summary table from the data model.

```
gross_margin = CALCULATE(SUM(transaction_table[net_sales]) - SUM(order_cost_summary[order_cost_ttl]))
```

<img width="373" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/b3184f8e-17a5-4962-8d32-d5d715cb47fd">

The last of the DAX calculations are for date related fields. These will be used to analyze temporal trends in the sales data. The first calculated field is to extract the calendar day of the week from the sale date field.

```
calendar_date = FORMAT(transaction_table[sales date], "dddd")
```

<img width="242" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/5b1bfd11-dc46-4c6b-a73f-3a333384c2eb">

The second to last of the date calculations is to extract the calendar month of the sales date data.

```
calendar_month = FORMAT(transaction_table[sales date], "MMMM")
```

<img width="220" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/64d57ee2-621d-44b1-a151-2c23829168ce">

The last date calculation is to extract the calendar year from the sales date.

```
calendar_year = FORMAT(transaction_table[sales date], "YYYY")
```

<img width="218" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/fbd2f874-1b81-4ab3-b99c-22ed1aaa4921">


The final product for the details table with all of the new calculated fields is shown below.

<img width="818" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/be9c5636-11e1-4f54-ae10-1840662f0b40">



Note that DAX is a very capable tool and it is able to perform much more complicated calculations that I have demonstrated in this project.

--------------------------------------------------------------------------------------------------

# create multiple dashboards

The first dashboard is a high level static view of the sales environment.

<img width="658" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/47567e3b-32a4-4b8e-b62a-fc9d735ffa72">


The second dashboard is a fully interactive dashboard to explore sales trends using key time based parameters.

<img width="665" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/301948fb-949e-42b9-ac91-69d60182a3ba">

The third dashboard is an interactive dashboard to explore the profitability by location and product line.

<img width="653" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/1782f362-2b8a-499a-bd2f-c03c439e4dbb">



-------------------------------------------------

# interpret the results




------------------------------------------------------------------------------------------

### Go back to my profile page
[garth-c profile page] (https://github.com/garth-c)

------------------------------------------------------------------------------------------------
