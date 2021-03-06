## Repository Info:

Repository Size: 6.5 MB

Code can be run using Visual Studio Code or Jupyter Notebook

Given player demographic and purchase history of a fictonal game, Heroes of Pymoli, various data frames were created to clean, merge, and analyze the data using the Pandas library.

![1](Images/1.png)

![2](Images/2.png)

## Heroes of Pymoli

![Fantasy](Images/Fantasy.png)

## My Data Results

### Player Count (total players)
* Import and read into csv file
```python
#add dependencies
import pandas as pd

#create variable to hold path for csv file
HOP_purchase_data = "Resources/HOP_purchase_data.csv"

#read csv file and store in pd frame
original_df = pd.read_csv(HOP_purchase_data)

#print head to view first 5 rows of dat and column headers
original_df.head()
```

* Create a DataFrame to count the total number of players
```python
#Total Players
#create variable to hold unique user names in SN column and find length of column for total unique player names
playerNames = len(original_df["SN"].unique())

#create new data frame for total players
totalPlayers_df = pd.DataFrame({"Total Players": [playerNames]})
totalPlayers_df
```
<div>
       <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;">
             <th></th>
             <th>Total Players</th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>0</th>
             <td>576</td>
           </tr>
         </tbody>
       </table>
       </div>

### Purchasing Analysis (Total)

* Number of Unique Items
* Average Purchase Price
* Total Number of Purchases
* Total Revenue
```python
#Purchasing Analysis (Total)
#use original df for purchasing Analysis (Total)
#create varibales to hold specific values

uniqueItems = len(original_df["Item Name"].unique())
totalPrice = sum(original_df["Price"])
averagePrice = totalPrice / len(original_df["Price"])
totalPurchases = len(original_df["Purchase ID"])

#create new data frame for Purchasing Analysis (Total)
purchasingAnalysis_df = pd.DataFrame({"Number of Unique Items": [uniqueItems],
                                     "Average Price": averagePrice,
                                     "Number of Purchases": totalPurchases,
                                    "Total Revenue": totalPrice})

# #format appropriate cells to currency
purchasingAnalysis_df["Average Price"] = purchasingAnalysis_df["Average Price"].map("${:,.2f}".format)
purchasingAnalysis_df["Total Revenue"] = purchasingAnalysis_df["Total Revenue"].map("${:,.2f}".format)

purchasingAnalysis_df
```
<div>
        <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;\">
             <th></th>
             <th>Number of unique Items</th>
             <th>Average Price</th>
             <th>Number of Purchases</th>
             <th>Total Revenue</th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>0</th>
             <td>179</td>
             <td>$3.05</td>
             <td>780</td>
             <td>$2,379.77</td>
           </tr>
         </tbody>
       </table>
       </div>

### Gender Demographics

* Percentage and Count of Male Players
* Percentage and Count of Female Players
* Percentage and Count of Other / Non-Disclosed
```python
#Gender Demographics

#create new DF with SN and Gender columns then drop duplicates
nameGender_df = original_df[["SN", "Gender"]]

#created variable for de deplicated df
uniqueGenders_df = nameGender_df.drop_duplicates()

#creat variables for values of genders
totalGenders = len(uniqueGenders_df)
malePlayers = uniqueGenders_df["Gender"].value_counts()['Male']
femalePlayers = uniqueGenders_df["Gender"].value_counts()['Female']
otherPlayers = uniqueGenders_df["Gender"].value_counts()['Other / Non-Disclosed']

#caluclate percentages
percentMale = round((malePlayers / totalGenders * 100), 2)
percentFemale = round((femalePlayers / totalGenders * 100), 2)
percentOther = round((otherPlayers / totalGenders * 100), 2)

#create new df for Gender Demographics
genderDemographics_df = pd.DataFrame([{"": "Male", "Total Count": malePlayers, "Percentage of Players": percentMale}, 
                                     {"": "Female", "Total Count": femalePlayers, "Percentage of Players": percentFemale}, 
                                    {"": "Other / Non-Disclosed", "Total Count": otherPlayers, "Percentage of Players": percentOther}])
#format appropriate cells to %
genderDemographics_df["Percentage of Players"] = genderDemographics_df["Percentage of Players"].map("{:,.2f}%".format)

#reset index to remove row #'s
reIndexedGenderDemographics_df = genderDemographics_df.set_index("")

reIndexedGenderDemographics_df
```
<div>
        <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;\">
             <th></th>
             <th>Total Count</th>
             <th>Percentage of Players</th>
           </tr>
           <tr>
             <th></th>
             <th></th>
             <th></th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>Male</th>
             <td>484</td>
             <td>84.03%</td>
           </tr>
           <tr>
             <th>Female</th>
             <td>81</td>
             <td>14.06%</td>
           </tr>
           <tr>
             <th>Other / Non-Disclosed</th>
             <td>11</td>
             <td>1.91%</td>
           </tr>
         </tbody>
       </table>
       </div>

### Purchasing Analysis (Gender)

* The below each broken by gender
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Gender
```python
#Purchasing Analysis (Gender)

#use groupby for data
genderPurchases = original_df.groupby(["Gender"])

#count total items
purchaseCount = genderPurchases["Purchase ID"].count()
#average price by each group
averagePurchase = genderPurchases["Price"].mean()
#total price for each gender
totalPurchase = genderPurchases["Price"].sum()

#calculate average purchase per person
personAveragePurchase = totalPurchase / playerNames  

#create a summary table of purchasing by gender
genderPurchases_df = pd.DataFrame({"Purchase Count": purchaseCount,
                                  "Average Purchase Price": averagePurchase.map("${:.2f}".format),
                                  "Total Purchase Value": totalPurchase.map("${:.2f}".format), 
                                  "Average Total Purchase per Person": personAveragePurchase.map("${:.2f}".format)
                                  })
genderPurchases_df
```
<div>
        <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;\">
             <th></th>
             <th>Purchase Count</th>
             <th>Average Purchase Price</th>
             <th>Total Purchase Value</th>
             <th>Average Total Purchase Per Person</th>
           </tr>
           <tr>
             <th>Age Group</th>
             <th></th>
             <th></th>
             <th></th>
             <th></th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>&lt;10</th>
             <td>23</td>
             <td>$3.35</td>
             <td>$77.13</td>
             <td>$4.54</td>
           </tr>
           <tr>
             <th>10-14</th>
             <td>28</td>
             <td>$2.96</td>
             <td>$82.78</td>
             <td>$3.76</td>
           </tr>
           <tr>
             <th>15-19</th>
             <td>136</td>
             <td>$3.04</td>
             <td>$412.89</td>
             <td>$3.86</td>
           </tr>
           <tr>
             <th>20-24</th>
             <td>365</td>
             <td>$3.05</td>
             <td>$1,114.06</td>
             <td>$4.32</td>
           </tr>
           <tr>
             <th>25-29</th>
             <td>101</td>
             <td>$2.90</td>
             <td>$293.00</td>
             <td>$3.81</td>
           </tr>
           <tr>
             <th>30-34</th>
             <td>73</td>
             <td>$2.93</td>
             <td>$214.00</td>
             <td>$4.12</td>
           </tr>
           <tr>
             <th>35-39</th>
             <td>41</td>
             <td>$3.60</td>
             <td>$147.67</td>
             <td>$4.76</td>
           </tr>
           <tr>
             <th>40+</th>
             <td>13</td>
             <td>$2.94</td>
             <td>$38.24</td>
             <td>$3.19</td>
           </tr>
         </tbody>
       </table>
       </div>

### Age Demographics

* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age Group
```python
#Purchasing Analysis by Age
countPurchases = ageGroup["Purchase ID"].count()

#average Purchase Price
averagePurchasePrice = ageGroup["Price"].mean()

#total purcahse value
totalPurchaseValue = ageGroup["Price"].sum()

#average total purchase per person
averagePersonPurchase = totalPurchaseValue / totalCountAge

#create new data frame with appropriste columns
purchaseAnalysisByAge = pd.DataFrame({"Purchase Count": countPurchases,
                                     "Average Purchase Price": averagePurchasePrice.map("${:,.2f}".format),
                                     "Total Purchase Value": totalPurchaseValue.map("${:,.2f}".format),
                                     "Average Total Purchase Per Person": averagePersonPurchase.map("${:,.2f}".format)})

purchaseAnalysisByAge
```
<div>
       <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;\">
             <th></th>
             <th>Purchase Count</th>
             <th>Average Purchase Price</th>
             <th>Total Purchase Value</th>
             <th>Average Total Purchase per Person</th>
           </tr>
           <tr>
             <th>Gender</th>
             <th></th>
             <th></th>
             <th></th>
             <th></th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>Female</th>
             <td>113</td>
             <td>$3.20</td>
             <td>$361.94</td>
             <td>$0.63</td>
           </tr>
           <tr>
             <th>Male</th>
             <td>652</td>
             <td>$3.02</td>
             <td>$1967.64</td>
             <td>$3.42</td>
           </tr>
           <tr>
             <th>Other / Non-Disclosed</th>
             <td>15</td>
             <td>$3.35</td>
             <td>$50.19</td>
             <td>$0.09</td>
           </tr>
         </tbody>
       </table>
       </div>

* Percent of players by age group
```python
#create bins for ages
ageBins = [0, 9.99, 14.99, 19.99, 24.99, 29.99, 34.99, 39.99, 150]
ageBinsNames = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#Slice data intogroupd using pd.cut
original_df["Age Group"] = pd.cut(original_df["Age"], ageBins, labels = ageBinsNames,)

#group data into bins
ageGroup = original_df.groupby("Age Group")

#count number of unique plapyers by age for age group
totalCountAge = ageGroup["SN"].nunique()

#percentage of players by age group
percentAgeGroup = (totalCountAge / playerNames) * 100

#create new data frame with appropriate columns
ageDemographics_df = pd.DataFrame({"Total Player Count": totalCountAge, "Percent of Players": percentAgeGroup.map("{:,.2f}%".format)})

ageDemographics_df
```
<div>
        <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;\">
             <th></th>
             <th>Total Player Count</th>
             <th>Percent of Players</th>
           </tr>
           <tr>
             <th>Age Group</th>
             <th></th>
             <th></th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>&lt;10</th>
             <td>17</td>
             <td>2.95%</td>
           </tr>
           <tr>
             <th>10-14</th>
             <td>22</td>
             <td>3.82%</td>
           </tr>
           <tr>
             <th>15-19</th>
             <td>107</td>
             <td>18.58%</td>
           </tr>
           <tr>
             <th>20-24</th>
             <td>258</td>
             <td>44.79%</td>
           </tr>
           <tr>
             <th>25-29</th>
             <td>77</td>
             <td>13.37%</td>
           </tr>
           <tr>
             <th>30-34</th>
             <td>52</td>
             <td>9.03%</td>
           </tr>
           <tr>
             <th>35-39</th>
             <td>31</td>
             <td>5.38%</td>
           </tr>
           <tr>
             <th>40+</th>
             <td>12</td>
             <td>2.08%</td>
           </tr>
         </tbody>
       </table>
       </div>

### Top Spenders

* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
  * SN
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
```python
#Top Spenders

#group purchases by SN
topSpenders = original_df.groupby("SN")

#count purchased items
spenderPurchaseCount = topSpenders["Purchase ID"].count()

#count purchased items
spenderPurchaseAverage = topSpenders["Price"].mean()

#Total Purchase Value
spenderPurchasesTotal = topSpenders["Price"].sum()

#create new data frame with appropriste columns
topSpenders_df = pd.DataFrame({"Purchase Count": spenderPurchaseCount, 
                               "Average Purchase Price": spenderPurchaseAverage.map("${:,.2f}".format),
                               "Total Purchase Value": spenderPurchasesTotal.map("${:,.2f}".format)})


#sort descending then format cells with correct values
formatedTopSpender = topSpenders_df.sort_values(["Total Purchase Value"], ascending=False).head()

formatedTopSpender
```
<div>
       <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;\">
             <th></th>
             <th>Purchase Count</th>
             <th>Average Purchase Price</th>
             <th>Total Purchase Value</th>
           </tr>
           <tr>
             <th>SN</th>
             <th></th>
             <th></th>
             <th></th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>Haillyrgue51</th>
             <td>3</td>
             <td>$3.17</td>
             <td>$9.50</td>
           </tr>
           <tr>
             <th>Phistym51</th>
             <td>2</td>
             <td>$4.75</td>
             <td>$9.50</td>
           </tr>
           <tr>
             <th>Lamil79</th>
             <td>2</td>
             <td>$4.64</td>
             <td>$9.29</td>
           </tr>
           <tr>
             <th>Aina42</th>
             <td>3</td>
             <td>$3.07</td>
             <td>$9.22</td>
           </tr>
           <tr>
             <th>Saesrideu94</th>
             <td>2</td>
             <td>$4.59</td>
             <td>$9.18</td>
           </tr>
         </tbody>
       </table>
       </div>

### Most Popular Items

* Identify the 5 most popular items by purchase count, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value
```python
#Most Popular Items

#group the df by item, id, and item name    
popular_items = original_df.groupby(["Item ID","Item Name"])

#calculate purchase count for each purchase id
purchase_count = popular_items["Purchase ID"].count()

#calculate total price for each group values
total_value = popular_items["Price"].sum()

#calculate item price by dividing total to count of purchase 
item_price = total_value / purchase_count

#create a df to display data
top_items = pd.DataFrame({"Purchase Count": purchase_count,
                         "Item Price": item_price.map("${:,.2f}".format),
                         "Total Purchase Value": total_value.map("${:,.2f}".format)})

#sort the data to get top 5 popular items
popular_df = top_items.sort_values(["Purchase Count"], ascending = False)
                                                          
popular_df.head()
```
<div>
        <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;\">
             <th></th>
             <th></th>
             <th>Purchase Count</th>
             <th>Item Price</th>
             <th>Total Purchase Value</th>
           </tr>
           <tr>
             <th>Item ID</th>
             <th>Item Name</th>
             <th></th>
             <th></th>
             <th></th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>92</th>
             <th>Final Critic</th>
             <td>13</td>
             <td>$4.61</td>
             <td>$59.99</td>
           </tr>
           <tr>
             <th>178</th>
             <th>Oathbreaker, Last Hope of the Breaking Storm</th>
             <td>12</td>
             <td>$4.23</td>
             <td>$50.76</td>
           </tr>
           <tr>
             <th>145</th>
             <th>Fiery Glass Crusader</th>
             <td>9</td>
             <td>$4.58</td>
             <td>$41.22</td>
           </tr>
           <tr>
             <th>132</th>
             <th>Persuasion</th>
             <td>9</td>
             <td>$3.22</td>
             <td>$28.99</td>
           </tr>
           <tr>
             <th>108</th>
             <th>Extraction, Quickblade Of Trembling Hands</th>
             <td>9</td>
             <td>$3.53</td>
             <td>$31.77</td>
           </tr>
         </tbody>
       </table>
       </div>

### Most Profitable Items

* Identify the 5 most profitable items by total purchase value, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value
```python
# Sort the data frame by total purchase value 
profitable_items = top_items.sort_values(["Total Purchase Value"], ascending = False)

# Display the new data frame
profitable_items.head()
```
<div>
        <table border=\"1\" class=\"dataframe\">
         <thead>
           <tr style=\"text-align: right;">
             <th></th>
             <th></th>
             <th>Purchase Count</th>
             <th>Item Price</th>
             <th>Total Purchase Value</th>
           </tr>
           <tr>
             <th>Item ID</th>
             <th>Item Name</th>
             <th></th>
             <th></th>
             <th></th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>92</th>
             <th>Final Critic</th>
             <td>13</td>
             <td>4.614615</td>
             <td>59.99</td>
           </tr>
           <tr>
             <th>178</th>
             <th>Oathbreaker, Last Hope of the Breaking Storm</th>
             <td>12</td>
             <td>4.230000</td>
             <td>50.76</td>
           </tr>
           <tr>
             <th>82</th>
             <th>Nirvana</th>
             <td>9</td>
             <td>4.900000</td>
             <td>44.10</td>
           </tr>
           <tr>
             <th>145</th>
             <th>Fiery Glass Crusader</th>
             <td>9</td>
             <td>4.580000</td>
             <td>41.22</td>
           </tr>
           <tr>
             <th>103</th>
             <th>Singed Scalpel</th>
             <td>8</td>
             <td>4.350000</td>
             <td>34.80</td>
           </tr>
         </tbody>
       </table>
       </div>