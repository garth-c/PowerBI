# PowerBI demo

Business purpose:
To analyze customer sales data for the purpose of increasing customer sales in the future. 

Project purpose:
In this Power BI demo project, I will present an interactive showcase of data visualization and business intelligence capabilities. Using Power BI, I create dashboards that offer valuable insights from a real-world dataset.

The project begins with data preparation and shaping, ensuring that the dataset is ready for intake into Power BI. Using Power BI's intuitive interface, I design visually useful charts, graphs, and tables to present key metrics and trends.

Specifically, I will demonstrate the basic functionality in PowerBI (desktop version only) to process data, create data models, reports, data visualizations, and dashboards. However, all of this would normally be published to Power BI Service so that others would be able to use the output. Using PowerBI Service, there are significantly more benefits available to a developer such as row level security, data sharing, app publishing, etc. In addition, this project is only a sampling of the capabilities of Power BI - this tool is very capable and it has the distinct advantage of being part of the Microsoft ecosystem as well as it is tightly coupled and well integrated with other Microsoft tools like Azure data science tools, SQL Server, etc. Also, I have my Microsoft Certified: Power BI Data Analyst Associate certification so this demo project only represents a sliver of what I am capable of developing with Power BI. 

Data description:
This data was a denormalized super set of data that I found on Kaggle.com. I normalized the data (separated) the transaction data (details data) and put it into a separate file as well as I then created several master file tables (fact data) and put them into separate files. All of the fact tables and the details table have keys in order to demo the star schema approach to data model building. Also, all of these files are Excel files and I imported them into PowerBI rather than use of the other data import methods. In a real business setting, considerations for incremental refreshing, direct querying, live connections, etc. would need to be evaluated given the objective of the project and the available data sources.  

For the fact table, I inserted an order number that is used as a key to easily connect the respective cost data for that specific order from a summarized order cost table. I also removed all of the extended amounts around sales totals and discounts in order to demonstrate the calculation functionality in DAX. I also rounded the units sold to an integer value (whole number) along with the order unit sales price to keep the math simple. The details table dimensions are 700x8 and the fact tables are all very small with only a up to 6 or 7 records each and a few columns - depending on the table.

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
+ create multiple dashboards & interpret the results

----------------------------------------------------------------------------------------

# read in the source data files with Power Query and the M language

The details table import is shown below. All of the data types are what they need to be and no data transformation will be needed.
<img width="719" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/2818cdc3-8716-43c5-a548-68a0ae367943">

To make the import process a little more dynamic, I created a parameter that has the path on my computer in it and I inserted that in the Source step in Power Query 'Source' step. The parameter is called 'local_path'. Note that the M language is very dynamic and many other features could have been used such query parameters, functions for repeated operations, pivot/unpivot, summary tables, etc. 

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

If there were sub-dimension tables for the various fact tables, then a snow flake schema would have been needed. 


The relationship between these tables is shown in the connectors and for the fact tables this is a many to 1 relationship. Below is a depiction of a many to 1 relationship for one of the fact tables to the detail tables. Since the details table will have many instances of the key value and the fact table will have only one instance of the key value, this is the many to 1 relationship that I am referring to. The only relationship in this data model that has a different association is the order cost summary table to the details table. Since the cost data is presumed to be from a different table altogether, a summarization of the aggregated costs relative to the specific order number is the relationship. So each processed order will have a summarize cost to associated with it.

<img width="343" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/2e9512aa-7e9b-43a3-962d-41b904c9d571">

A screen shot of the imported data is shown below for reference.

<img width="476" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/f6d4e5ed-5623-4f11-b7eb-66710908329a">

----------------------------------------------------------------------------------------------------

# create the table calculations with DAX

There are 4 calculated fields that need to be determined using DAX. Note that there are other ways of doing this too such as with a Measure, etc. but I wanted to showcase how DAX works as a calculated column instead. Using the calculated column approach does increase the size of the model but since this is a small project, this wasn't a big concern.  

The first calculated column is to extend the sales total. This is the unit cost * the unit price. The DAX code to this is below.

```
extend_sales = CALCULATE(SUM('transaction_table'[unit price]) * SUM(transaction_table[units sold]))
```

<img width="347" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/e4884e4d-b436-4b9b-b316-94ebbf633dae">

The next calculated column to code the discount total. This is applying the discount rate (as a percent of the extended sales amount) from the discount rate fact table from the data model to the extendes sales amount that I just calculated above.

```
discount_amt = CALCULATE(SUM(transaction_table[extend_sales]) * SUM(discount_master[disc_prcnt]))
```

<img width="355" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/6d562ea1-927e-40fa-9b3b-b52a824532df">

The next calculated column is to determine the net sales amount which is the extended sales from above less the calculated discount total also calculated above.
```
net_sales = CALCULATE(SUM(transaction_table[extend_sales]) - SUM(transaction_table[discount_amt]))
```

<img width="344" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/a92ed285-5175-4afd-b56e-372397adb39b">

The last calculated column is to determine the gross margin for each sale. This is the net sales from above less the aggregated total order costs from the order cost summary table from the data model.

```
gross_margin = CALCULATE(SUM(transaction_table[net_sales]) - SUM(order_cost_summary[order_cost_ttl]))
```

<img width="373" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/b3184f8e-17a5-4962-8d32-d5d715cb47fd">

The last of the DAX calculations are for date related fields. These will be used to analyze temporal trends in the sales data. The first calculated column is to extract the calendar day of the week from the sale date field.

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


The final product for the details table with all of the new calculated columns is shown below.

<img width="818" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/be9c5636-11e1-4f54-ae10-1840662f0b40">


Another approach is to create a dedicated date table using DAX (marking them as Date tables) and then make copies of it in the semantic model as needed to connect to other tables as appropriate. Or if only one master date table is created to then create multiple connections to other tables in the semantic model and then code the USERELATIONSHIP function for DAX measures as needed. Any of these approaches would work just fine and then selecting the best one for the circumstances and maintainability becomes the decision to make. Note that DAX is a very capable tool and it is able to perform much more complicated calculations that I have demonstrated in this project.

--------------------------------------------------------------------------------------------------

# create multiple dashboards & interpret the results

The first dashboard is a high level static view of the sales environment. This dashboard shows an analysis and decomposition of sales data by specific parameters that would be useful to management. This dashboard breaks downs sales by customer type, customer country, as well as it displays sales (gross and net) over time to look for visible trends. In addition, it displays the discount amount and gross margin over time to also look for obvious visible trends and the relationship of discount and gross marging.

<img width="658" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/47567e3b-32a4-4b8e-b62a-fc9d735ffa72">


The second dashboard is a fully interactive dashboard to explore sales trends using key time based parameters. Management would be able to select whichever sector of the business that was being evaluated and quickly evaluate the gross sales over time as well as which calendar day of the week had the most sales. All of the filters (slicers) are synched work together on this dashboard so drilling down into a segment or a customer type is very simple. In addition, bookmarks and buttons could have been used to create custom views for the end users if needed. Also, PowerBI supports drill downs and drill throughs which could have been implemented in this dashboard if needed. 

<img width="665" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/301948fb-949e-42b9-ac91-69d60182a3ba">

The third dashboard is an interactive dashboard to explore the profitability by location and product line. This dashboard would be a key one to investigate for management since the focus on on gross margin which then relates directly to the overall sales order level of profitability. The dashboard is also fully interactive by customer type and it displays gross margin by country and product as well as it tracks the overall gross margin compared to the goal. The last part of this dashboard has a card that displays the overall totals for gross sales, discount amount and net sales. The interactivity is very important to use in drilling down order level profitability by customer type. 

<img width="653" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/1782f362-2b8a-499a-bd2f-c03c439e4dbb">

-------------------------------------------------

### Go back to my profile page
[garth-c profile page] (https://github.com/garth-c)

------------------------------------------------------------------------------------------------
