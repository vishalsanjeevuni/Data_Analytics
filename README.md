# Marketing Data Analytics

The purpose of this project is to analyze marketing data from kaggle, Get acquainted with the data, Clean the data so it is ready for analysis, Develop some questions for analysis, and Analyze variables within the data to gain patterns and insights on these questions

### Data Collection

Files : `marketing_data`

For my data mining I used one source: [kaggle](https://www.kaggle.com/jackdaoud/marketing-data)

Information regarding the features for the data are located in the `Column` section on the website.

### Data Information


- There are 28 columns and 2240 rows.
- The name and datatype of each column -- most values are integers in this dataset.
- The income column has missing data, values that are not integers or floats, and an extra space in the column name, so some cleaning will be necessary for this - column prior to conducting EDA.
- The column names could be renamed for more consistency.
- Some basic summary statistics on each of the numerical variables.


### Data Cleaning

The `Income` column needs some cleaning. Renamed the column names for overall consistency. To do this, the following is done:
* Changed all columns in `snake case format` using regex and list comprehension
* Changed `Income` values to floats
* Changed the values as floats

The `Income` distribution is then looked at using boxplots. Since there is one large outlier, it is removed from the `marketing_data`. Next, the missing values are replaced with the mean income using the `.mean()` method.

This boxplot showed a major outlier on the right, so I have removed it by limiting income variable from the dataset.

![Screen Shot 2021-12-22 at 3 25 53 PM](https://user-images.githubusercontent.com/26355917/147151296-8b93eb7e-74e8-49d1-a695-a9d3370a035c.png)

As seen above after removing the outlier, the distribution is more symmetric. There are still some outliers; however, with not major skewness or huge outliers remaining, the income variable is ready for analysis.

### Adding an age Column 

The `marketing_data` DataFrame contains a `year_birth` column; however, a column with the age of each customer may be easier for analysis. Because of this, the following is done:

* Added a new column called `age` by subracting each value of `year_birth` from 2020 (the year the dataset is from). 
* Removed the outliers in `age` column by limiting `age` that could affect the analysis.
* After removing the major outliers the age distribution is symmetric and ready for analysis.

![Screen Shot 2021-12-22 at 3 40 20 PM](https://user-images.githubusercontent.com/26355917/147152585-e1799267-5c5d-4dde-952d-a95c6981f912.png)

### Checking the Education Variable 

The education variable is another column to focus on in the analysis. Plotted a boxplot to see if any cleaning is needed before EDA. There is no missing data or other issues


![Screen Shot 2021-12-22 at 3 48 31 PM](https://user-images.githubusercontent.com/26355917/147153368-52701385-7abc-4916-a3ae-a6a3495459d1.png)


## Exploratory Data Analysis

After some data cleaning and tidying, the DataFrame is ready for EDA. The following are the independent variables to focus on in the analysis:
* `income`
* `education`
* `age`

The goal is to see how these independent variables associate with the following dependent variables:
* `mnt_wines`
* `mnt_fruits`
* `mnt_meat_products`
* `mnt_fish_products`
* `mnt_sweet_products`  
* `mnt_gold_products`  
* `num_deals_purchases`
* `num_web_purchases`  
* `num_catalog_purchases`  
* `num_store_purchases`

The hope is to the answer the following question:
* Does a shopper's income, education level, and/or age relate to their purchasing behavior?

### Big Picture

In order to observe the dataset as a whole used, `DataFrame.hist()` To view of all numerical variables in the distribution. Most of the amount bought and number purchased variables are skewed right and have similar distributions.

Next, checked correlations between all numerical variables using a heat matrix. The heat matrix shows that `income` has the strongest association with numerous variables. Interestingly, it showed that `age` may not be a huge factor overall.
The overview below shows that the purchase behavior columns are all skewed to the right.

![Screen Shot 2021-12-22 at 3 59 50 PM](https://user-images.githubusercontent.com/26355917/147154433-f7da3a11-2800-4e17-aec8-3fa99d26dce7.png)


The table of correlations below does not offer much help as there are too many numbers to read through. However, the heat map shows that income will be the major variable to focus on in the analysis.
![Screen Shot 2021-12-22 at 4 02 59 PM](https://user-images.githubusercontent.com/26355917/147154758-619f10b2-91ca-41fe-a1b5-eeaca13b27e7.png)

### Purchasing Behavior by Income
A `for` loop is used to see the relationship bewteen `income` and each `num_{type}_purchases` variable. The `hue` parameter with the `education` variable is used to see if there are any patterns that can be deciphered between `education` and `num_{type}_purchases`. 

First scatterplots are used and then regression plots are used for this analysis.

![Screen Shot 2021-12-22 at 4 08 22 PM](https://user-images.githubusercontent.com/26355917/147155321-0e558d85-8cdb-46d2-b4e3-c5dff50c7f6a.png)

There is a fairly strong, positive linear relationship between `income` and the following three variables:
* `num_catalog_purchases`
* `num_store_purchases`
* `num_web_purchases`

Between `income` and `NumDealsPurchaes`, however, there is no obvious relationship. It appears there might be a weak, negative linear relationship but it is not strong enough to be confident. It is also difficult to decipher any patterns associated with `education` in the plots, so further analysis will be done on this variable.

![Screen Shot 2021-12-22 at 4 09 40 PM](https://user-images.githubusercontent.com/26355917/147155485-96edf1e4-57d5-431a-9299-aebe7307dce7.png)

To get a better look at the linear relationships, `.regplot()` was used. `num_catalog_purchases` and `num_store_purchases` have the strongest positive, linear relationship with `income`. 

These plots also show that `income` and `num_deals_purchases` have a linear, negative relationship; however, it is still too weak to be conclusive.

For some further analysis, a new column in the DataFrame called `total_purchases` is added to the `marketing_data` DataFrame. It is the sum of all `num_{type}_purchases` variables. The same analysis with `.scatterplot()` and `.regplot()` plot methods is done on this new column

![Screen Shot 2021-12-22 at 4 10 58 PM](https://user-images.githubusercontent.com/26355917/147155622-aedab024-2a9e-44ec-9d09-59a5a242515f.png)

The overall relationship between `income` and `total_purchases` is strong and linear. Unfortunately, it is still hard to decipher any relationship with the `education` and `total_purchases` as the points are scattered randomly across the plot.

### More Purchasing Behavior by Income

The following analysis is very similar as before. However, instead of looking at the relationship between `income` and `num_{type}_purchases`, this analysis will be looking at the relationship between `income` and `mnt_{type}_products`. The steps for this analysis will essentially be the same.

![Screen Shot 2021-12-22 at 4 12 17 PM](https://user-images.githubusercontent.com/26355917/147155721-523ea609-d217-4504-9915-6b386eb31a66.png)

These plots all show a positive relationship between `income` and each `mnt_{type}_products` variable. However, there is not enough visual evidence to see that it is linear. For further analysis, The *log* scale of the the `income` variable and the `mnt_{type}_products` variables are plotted.

![Screen Shot 2021-12-22 at 4 13 11 PM](https://user-images.githubusercontent.com/26355917/147155824-275e2ac2-68ec-4482-8061-dd786157a3e6.png)

With the *log* scaled variables, it is easy to see there is an fairly strong linear, positive relationship between the variables across the board. It is still hard to see how education plays a role, however.

### Purchasing Behavior by Education and Income
A seaborn method called `.FacetGrid()` is used to see how education effects purchasing behavior along with `income`. It gives a much clearer picture than the `hue` parameter in previous plots. In this analysis, a loop and a dynamic Python variable are used to plot six sets of `.FacetGrid()` plots.

After observing the plots detailing the relationship between income, education, and purchasing behavior, the following can be seen:
* This store does not have many shoppers with a `Basic` education level.
* Regardless of the shopper's educational level, there is a positive, linear relationship for each `mnt_{type}_products`.
* `mnt_wines` has the strongest positive, linear relationship with `education` by `income`.

### Purchasing Behavior by Age

The last main variable in our analysis plan is `age`. The `.scatterplot()` method is used to see if there is any relationship bewteen `age` and any purchasing behavior variables. The initial analysis showed no evidence of relationship as shown in all the graphs below. The graphs shown are:
* `total_purchases` vs. `age`
* `mnt_{type}_products` vs. `age`
* `num_{type}_purchases` vs. `age`

The process used to plot each one of these graphs is very similar to the one outlined in the Purchasing Behavior by income section.

![Screen Shot 2021-12-22 at 4 17 17 PM](https://user-images.githubusercontent.com/26355917/147156228-6c3b6928-5218-4a27-a044-c27393bf03a7.png)

It is hard to see any relationship between `age` and `total_purchases` in this plot.

To do further analysis on the `age` variable, A new column called `age_group` is added to `marketing_data`. It contains the following categories of ages:
* `18 to 35`
* `36 to 50`
* `51 to 70`
* `71 and Older`

![Screen Shot 2021-12-22 at 4 18 38 PM](https://user-images.githubusercontent.com/26355917/147156341-955d979b-a569-41d3-b04d-f7dd12a39205.png)

The `age_group` variable proved to be much more useful quickly as a bar chart showed that `36 to 50` and `51 to 70` year-old age groups dominated shopping at the store.

To take the analysis further, a new DataFrame is created, which only has information about shopper age (`age` and `age_group`) and the total purchase amounts each age group buys (`mnt_{type}_products`). This new DataFrame will have `age_groups` as row data to make plotting a grouped bar graph easier.

![Screen Shot 2021-12-22 at 4 19 28 PM](https://user-images.githubusercontent.com/26355917/147156437-c49d1cdb-edd6-4f81-b3dc-720e5a4e297e.png)

Across the board, `age_group` does not seem to effect purchasing habits. Wine is the most popular bought item for each age group followed by meat products. The least popular bought item is fruits for each age group. The next analysis of interest is to see if `age_group` affects how many items customers buy each time. 



![Screen Shot 2021-12-22 at 4 20 35 PM](https://user-images.githubusercontent.com/26355917/147156511-5e05d50d-cea7-403c-a0a2-1b0e1e29e248.png)

This chart yields some very interesting insights. Here are some notable ones:
* `18 to 35` and `71 and Older` age groups tend to be the least interested in deals.
* On average, `71 and Older` age group customers tend to shop the most online, in store, and through the catalog.
* `36 to 50` and `51 to 70` age groups are interested in deals. Most likely this is because they receive more deals since they have more loyal customers.

This information could be super helpful for a marketing department as strategies could be used to increase `36 to 50` and `71 and Older` customers for the store.

### Conclusion 
#### Findings Overview

It has been shown `income` has the strongest relationship with purchase behavior of customers. However, interesting insights about `education` and `age` along with `age_group` have still been noted. These insights would be very helpful to how this store markets deals to their customers and prices items, such as wine since higher income groups tend to dominate alcohol sales. There is also opportunity to increase market to the `18 to 35` and `71 and Older` age groups to drive products sales. 

