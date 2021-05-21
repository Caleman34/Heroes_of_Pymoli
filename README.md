## Heroes of Pymoli

![Fantasy](Images/Fantasy.png)

Congratulations! After a lot of hard work in the data munging mines, you've landed a job as Lead Analyst for an independent gaming company. You've been assigned the task of analyzing the data for their most recent fantasy game Heroes of Pymoli.

Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. As a first task, the company would like you to generate a report that breaks down the game's purchasing data into meaningful insights.

## Requirements, Code and Results
Your final report should include each of the following:

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
           <tr>\
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