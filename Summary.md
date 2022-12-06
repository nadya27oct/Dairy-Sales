### Goal 

Predict an inventory order system for dairy products.

#### Overview  & Findings from EDA

We have daily sales data for *81 products* from January 2021 to mid October 2022.<br>

**Size Variable** 
Analysis on size variable showed that most sold sizes are 32 oz and 64 oz. But product sizes that sell in small volumes bring in a higher per unit margin.

**Price** 
Products are priced ranging from $0.49 to $12.99. This is expected as the grocery store sells regular milk to expensive cheese.
No clear relationship between price and sales as products priced at $6.99 still contributes a heavily to unit sales.

**Outliers**
Two main outliers. One product - upc 4190007663 - costs $14 on 1 day. The neighboring days for this product costs at about $1.50.
Upc 1570010320 recorded 32 units of sales on June 21st, 2021, when the same product has significantly low sales for neighboring days. <br>
I removed the outliers for some visualizations. But without the business context, I did not remove outliers in final model.

**Time Series Patterns**
There are some seasonaly patterns within a year in daily sales, especially around January to early June.<br>
Sales volume is significantly high in summer months.<br>
No clear trends or seasonality in sales plots against month, week of day, day of month.<br>

**Product Level Metrics**
We computed different metrics at the product level to get an understanding of the data. About half of products have less than 100 units of data across all weeks.<br>
Some products have very sparse weeks of sales, selling in Summer 2021 and no record until Summer 2022.<br>
These factors were considered when picking which products are eligible for modeling. We did not want to include products with low sales as they will give us a higher forecast error.<br>
As this is the first version of forecasting model, we will only include a subset of products with most historical data. <br>

#### Approach
We will use historical sales data to build an inventory order system at the product week level. 
Since daily sales data contains mostly 1-3 units, it will be difficult to use daily data. Therefore, I aggregated at the product weekly level.<br>
We will split data into training and test set and use historical sales (data from training period) to forecast sales for the selected testing period. <br>
Notebook lists all criteria used for picking the products. <br>

#### Current Models

Used rolling 4 week as base model to compare the model built.<br>
Used Random Forest tree as this captures all complex features, especially given this dataset did not contain a lot of features and gave a better accuracy than rolling 4 weeks average. <br>
Disadvantage of this model is it is difficult to explain to non-technical stakeholders unlike a simple regression model.


#### Additional Data
Inventory data. <br>
Data on promotions and discounts. <br>
Cost of holding inventory and cost of ordering <br>
Minimum and maximum products that can be held in shelf <br>
Holiday events <br>
Product Category <br>

#### Additional Work to Improve Model
The current **inventory order system** only use forecast sales as a proxy for how much inventory to order.<br>
Ideally, after forecasting sales, we use business constraints such as inventory data to predict optimal inventory level.

Long Term - Applying hierarchical time series forecasting. Aggregated sales at a category week level, using whole milk weekly sales to predict sales at individual product level.<br>