
## Housing-Overview-Data-Analysis Dashboard

Dashboard Link : https://app.powerbi.com/groups/me/reports/247882e4-8d01-43d5-9c46-15d0bf5025c3/7fc4bfef774b45bdc506?redirectedFromSignup=1&experience=power-bi

## Problem Statement

This dashboard helps to analyze housing market trends and real estate performance. It provides insights into sales growth, pricing trends, regional performance, and factors influencing property prices. Through different visualizations, it helps understand how house types, regions, and economic indicators impact the market, & thus supports better investment and business decisions.

Since, housing prices and sales fluctuate over time, thus analysis is required to identify trends and patterns.

Also since regional and economic factors affect property value, thus it is important to track key metrics for better decision-making.


### Steps followed

Step 1 : Open Google BigQuery, Add a file(local), create dataset, create a table and load the data to Google BigQuery.
Step 2 : Created a copy of the table named 'Test' in SQL Google Bigquery

Step 3 : Updated 'Square Meter Column(sqm)' value to '100' for all those cases where no_rooms is '3'. 

Step 4 : Load data into Power BI Desktop.

Step 5 : Open power query editor & check "column distribution", "column quality" & "column profile".

Step 6 : Select "column profiling based on entire dataset".

Step 7 : Data cleaning and transformation:

(1) In 'city' column NULL values were replaced by UNKNOWN.

(2) In 'dk_ann_infl_rate%' NULL values were replaced by 1.85(because this values occurs more often).

(3) In 'yield_on_mortgage_credit_bonds%' NULL values were replaced by 1.47(because this values occurs more often).

Step 8 : In reprot view, under visualization pane, format your report page, canvas background image was added on page named 'Housing market Overview'


Step 9 : KPIs, charts and DAX measures were created for:

# Housing Market Overview

      (1) YOY_Sales_Growth(DAX)
      VAR CurrYearSales = 
              CALCULATE(SUM(Housing[purchase_price]),
                     YEAR(Housing[date]) = YEAR(MAX(Housing[date])))

      VAR PrevYearSales = 
                CALCULATE(SUM(Housing[purchase_price]),
                 YEAR(Housing[date]) = YEAR(MAX(Housing[date]))-1)


      RETURN
           IF(PrevYearSales<>0, (CurrYearSales-PrevYearSales)/PrevYearSales, BLANK()) 

    * A 'Line chart' was created to represent "YOY Sales Growth by Sales Type" ('YOY_Sales_Growth'(y-axis) and 'Sales_type'(x-axis))

     (2) A new column was created using 'Purchase_Price' and '%_change_between_offer_and_purchase' named:
     Offer Price(DAX) 
     (100*Housing[purchase_price])/(100-Housing[%_change_between_offer_and_purchase]) 

![offer price](https://github.com/Abhisingh-code/Uploadingimages/blob/52f957dc3728530b32144c4b02f1c10b17fb4e49/OP2-PJ2.jpg)
 
    * A 'Scatter plot chart' was created to represent "Offer Price v/s Purchase Price" ('Offer Price'(don't summarize)(x-axis)  and 'Purchase_Price'(don't summarize)(y-axis))
      and (Zoom Sliders) were also added in this chart.

     (3) Median Sales Price Change(DAX) 
      VAR CurrMedianPrice = 
        MEDIANX(FILTER(Housing,
        YEAR(Housing[date].[Date]) =
        YEAR(MAX(Housing[date].[Date]))),
        Housing[purchase_price])

      VAR PreMedianPrice = 
        MEDIANX(FILTER(Housing,YEAR(Housing[date].[Date]) =
        YEAR(MAX(Housing[date].[Date]))-1),
        Housing[purchase_price])


    RETURN
         IF(PreMedianPrice<>0,(CurrMedianPrice-PreMedianPrice)/PreMedianPrice,BLANK())

    * A 'Stacked bar chart' was created to represent "Median Sales Price Change by Region" ('Region'(don't summarize)(x-axis)  and 'Median Sales Price Change'(don't summarize)(x-axis)).

     (4) Units Sold in Latest Year & Quarter(DAX)
            CALCULATE(DISTINCTCOUNT(Housing[house_id]),
            YEAR(Housing[date]) = YEAR(MAX(Housing[date])) &&
            QUARTER(Housing[date]) = QUARTER(MAX(Housing[date]))) 
 
    * A 'Card' visual was created to represent "Units Sold(Latest Yr & Qtr)" ('Units Sold in Latest Year & Quarter'(Values)
![12 Month Sales](https://github.com/Abhisingh-code/Uploadingimages/blob/0d6cb8246e334453b18b618335974495c286c4cf/12MS-PJ2.jpg)
    
    (5) Last 12 Month Sales(DAX)
          CALCULATE(SUM(Housing[purchase_price]),
          DATESINPERIOD(Housing[date],MAX(Housing[date]),-12,MONTH))

    * A 'Card' visual was created to represent "12 Month Sales" (' Last 12 Month Sales'(Values)
![Units Sold Latest yr&Qtr](https://github.com/Abhisingh-code/Uploadingimages/blob/0d6cb8246e334453b18b618335974495c286c4cf/US(LY%26Q)_PJ2.jpg)
# Sales Performance
 
An image was added in the background

    (1) Sales by Region(DAX) 
            CALCULATE(SUM(Housing[purchase_price]),ALLEXCEPT(Housing,Housing[region]))

    * A 'Stacked bar chart' was created to represent "Sales by Region" ('Sales by Region'(x-axis)  and 'Region'(y-axis))

    (2) TotalYTD Sales(DAX)
           TOTALYTD(SUM(Housing[purchase_price]),Housing[date].[Date])

    * A 'Table' visual was created to represent ("date" for Year, Quarter, Month, Day, "TotalYTD Sales", "Purchase_Price"(Sum) (Columns)) 

    (3) Average Price SQM(DAX)
          AVERAGE(Housing[sqm_price])

    * A 'Donut chart' was created to represent "Average Price SQM by Region" ('Region'(Legends) and 'Average Price SQM'(Values))
 
    (4) A column was created named: 
          Age(DAX)
            ABS(YEAR(Housing[date].[Date]) - Housing[year_build])
![AGE](https://github.com/Abhisingh-code/Uploadingimages/blob/50deca3886b274b7522ddde2684d36b287d24179/AGE-PJ2.jpg)

     * A 'Key Influencers' visual was added to Analyze ('Purcahse_price'(Analyze) and 'Age'(Don't Summarize) (Explain by)) 

    (5) Offer to SQM Ratio(DAX)
            DIVIDE(SUM(Housing[Offer Price]),SUM(Housing[sqm]))

    *  A 'Stacked bar chart' was created to represent "Offer to SQM Ratio by Sales Type"('Sales_type'(y-axis)  and 'Offer to SQM Ratio'(x-axis))

# House Type Analysis

(1) A 'Clustered bar chart' was created to represent "Avg Offer/Purchase Price by House Type" ('house_type'(y-axis) and 'Offer Price'(Average) 'Purchase Price'(Average) (x-axis))

(2) A 'Clustered bar chart' was created to represent "Avg Inflation/Interest/Yield by House Type" ('house_type'(y-axis) and 'Inflation'/'Interest'/'Yield'(Average) (x-axis))

'dk_ann_infl_rate%'   (Inflation)

'nom_interest_rate%'  (Interest)

'yield_on_mortgage_credit_bonds%' (Yield)

(3) A 'Line and Stacked column chart' was created to represent "Avg SQM/SQM Price by House Type" ('house_type'(x-axis) and 'SQM'(Average)(column y-axis) and 'SQM'(Average)(Line y-axis))


Step 10 : Slicers were added on House Type Analyses page for:

(a) AREA
(b) CITY
(c) SALES TYPE
(d) REGION

and search option was also provided under the slicers. 

Step 11 : Report published to Power BI Service.

Snapshot of Dashboard (Power BI Service)

![Page 1](https://github.com/Abhisingh-code/Uploadingimages/blob/c438c73d4afbdc3bfea596ed8563157ed336ddad/HMO(2)-PJ2.jpg)

![Page 2](https://github.com/Abhisingh-code/Uploadingimages/blob/cb826aaca39468d67f754fc2caa01d301ad194ae/SP-PJ2.jpg)

![Page 3](https://github.com/Abhisingh-code/Uploadingimages/blob/cb826aaca39468d67f754fc2caa01d301ad194ae/HTA-PJ2.jpg)


## Insights

A multi-page report was created on Power BI Desktop & published to Power BI Service.

Following inferences can be drawn:

[1] Sales Growth by Type

Sales growth varies across sales types, with some showing positive and others negative trends.

    thus, different sales strategies perform differently.  

[2] Offer vs Purchase Price

Offer price and purchase price show noticeable differences.

    thus, negotiation plays a key role in transactions.  

[3] Regional Performance

Regions like Zealand and Jutland contribute significantly to total sales.

    thus, regional demand varies.  

[4] Units Sold & Revenue

Units sold and total sales indicate strong market activity (billions in sales).

    thus, housing market operates at large scale.  

[5] Price per SQM

Price per square meter differs across regions.

    thus, location impacts property value.  

[6] House Type Analysis

Farm and Villa properties have higher average prices compared to others.

    thus, property type influences pricing significantly.  

[7] Key Influencers

Factors like age group and property characteristics influence purchase price.

    thus, multiple variables impact housing prices.  

