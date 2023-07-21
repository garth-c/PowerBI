# PowerBI demo

Project purpose:
Demonstrate the basic functionality in PowerBI to process data, create data models, reports, data visualizations, and dashboards. 

Data description:
This data was a denormalized super set of data that I found on Kaggle.com. I separated the transaction data (details data) and put it into a separate file. I then created several master file tables (fact data) and put them into separate files. All of the fact tables and the details table have keys in order to demo the star schema approach to data model building. Also, all of these files are Excel files and I converted all of the relevant data to a table format in order to import them into PowerBI. 

For the fact table, I inserted an order number that is used as a key to easily connect the respective cost data for that specific order from a summarized order cost table. I also removed all of the extended amounts around sales totals and discounts in order to demonstrate the calculation functionality in DAX. I also rounded the units sold to an integer value along with the order unit sales price to keep the math simple. The details table dimensions are 700x8 and the fact tables are all very small with only a up to 6 or 7 records each and a few columns - depending on the table.

Overall, I normalized a denormalized data set to demonstrate how this would work in reality as well as I modified several key aspects of the source data to demonstrate multiple features within PowerBI. 

<img width="190" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/4da7ac15-ac6b-4ef1-948b-188c4e71d29a">

A snippet of the details table is below:
<img width="656" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/be6cff1c-c057-4335-a4da-701fb0750372">

A snippet of a typical fact table layout is below:
<img width="155" alt="image" src="https://github.com/garth-c/PowerBI/assets/138831938/cfe2ed42-8f0a-465b-95ae-4dd1a77344be">

---------------------------------------------------------------------------------------

# road map for this project
+ read in source data files with Power Query and the M language
+ create the data model
+ create the table calculations in DAX
+ create a report and a dashboard
+ interpret the results

----------------------------------------------------------------------------------------

















