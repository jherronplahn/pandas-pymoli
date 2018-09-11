
### Heroes Of Pymoli Data Analysis
* Of the 780 purchases by players, the vast majority of purchases are made by males (83.59%). Although females only comprised 14.49% of purchases, their average purchase price was higher than males. (Females: \$3.20 vs Males: \$3.02).


* Our peak age demographic for purchases falls between 20-24 age range (44.79%), with the 15-19 age range coming in second (17.44%). While those in the <10 age range comprise only 2.95% of purchases, their average purchase price is the second highest at \$3.35.


* Based on the purchase data provided, the "Final Critic" item has both the highest number of purchases and highest total purchase value. 

-----

### Import and Read


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# Raw data file
file_to_load = "Resources/purchase_data.csv"

# Read purchasing file and store into pandas data frame
purchase_data = pd.read_csv(file_to_load)

purchase_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>



## Unique Player Count

* Display the total number of unqiue players that made purchases



```python
# Total players - find unqiue number of players
unique_players = len(purchase_data["SN"].value_counts())
total_players = {"Total # of unqiue players that made purchases": [unique_players]}
total_players_df = pd.DataFrame(total_players)
total_players_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total # of unqiue players that made purchases</th>
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



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
# Number of Unqiue Items
num_unq = len(purchase_data["Item ID"].value_counts())

# Average Price
avg_price = (purchase_data["Price"].mean())

# Number of Purchases
num_purch = (purchase_data["Price"].count())

# Total Revenue
total_rev = (purchase_data["Price"].sum())

# Create dataframe
purch_overview = {"Number of Unique Items": [num_unq], "Average Price":[avg_price], "Number of Purchases":[num_purch], "Total Revenue":[total_rev]}
purch_overview_df = pd.DataFrame(purch_overview)

# Format dataframe columns
purch_overview_df["Average Price"] = purch_overview_df["Average Price"].map("${:.2f}".format)
purch_overview_df["Total Revenue"] = purch_overview_df["Total Revenue"].map("${:.2f}".format)

purch_overview_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed





```python
gender_data = purchase_data.groupby(["Gender"])
gender_data.count()

# Total Count by gender
male = len(purchase_data.loc[purchase_data["Gender"]=="Male", :])
female = len(purchase_data.loc[purchase_data["Gender"]=="Female", :])
non_dis = len(purchase_data.loc[purchase_data["Gender"]=="Other / Non-Disclosed", :])


# Percent of Players by gender
percent_male = (male/(male + female + non_dis)) * 100
percent_female = (female/(male + female + non_dis)) * 100
percent_non = (non_dis/(male + female + non_dis)) * 100


# Create dataframe
gender_data_df = pd.DataFrame(
[[percent_female, female],
[percent_male, male],
[percent_non, non_dis]],
index=["Female","Male", "Other / Non-Disclosed"],
columns=["Percentage of Players", "Total Count"])

# Format dataframe columns
gender_data_df["Percentage of Players"] = gender_data_df["Percentage of Players"].map("{:.2f}%".format)
gender_data_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>14.49%</td>
      <td>113</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>83.59%</td>
      <td>652</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.92%</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
# Grouby from previous code bloack
gender_data.count()

# Purchase COUNT calculated in previous code block -- variables are: "female", "male", and "non_dis" and used below.

# Average Purchase Price
female_avg_price = purchase_data.loc[purchase_data["Gender"]=="Female", "Price"].mean()
male_avg_price = purchase_data.loc[purchase_data["Gender"]=="Male", "Price"].mean()
non_avg_price = purchase_data.loc[purchase_data["Gender"]=="Other / Non-Disclosed", "Price"].mean()

# Total Purchase Value
female_tot_val = purchase_data.loc[purchase_data["Gender"]=="Female", "Price"].sum()
male_tot_val = purchase_data.loc[purchase_data["Gender"]=="Male", "Price"].sum()
non_tot_val = purchase_data.loc[purchase_data["Gender"]=="Other / Non-Disclosed", "Price"].sum()

# Average Purchase Total Per
female_avg_tot_per = female_tot_val / female
male_avg_tot_per = male_tot_val / male
non_avg_tot_per = non_tot_val / non_dis

# Create dataframe
per_gender_data_df = pd.DataFrame(
[[female, female_avg_price, female_tot_val, female_avg_tot_per],
[male, male_avg_price, male_tot_val, male_avg_tot_per],
[non_dis, non_avg_price, non_tot_val, non_avg_tot_per]],
index=["Female","Male", "Other / Non-Disclosed"],
columns=["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Avg Purchase Total Per Person"])

# Format dataframe columns
per_gender_data_df["Average Purchase Price"] = per_gender_data_df["Average Purchase Price"].map("${:.2f}".format)
per_gender_data_df["Total Purchase Value"] = per_gender_data_df["Total Purchase Value"].map("${:.2f}".format)
per_gender_data_df["Avg Purchase Total Per Person"] = per_gender_data_df["Avg Purchase Total Per Person"].map("${:.2f}".format)

per_gender_data_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Purchase Total Per Person</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$3.35</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
# Establish bins for ages
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

purchase_data["Age"] = pd.cut(purchase_data["Age"], age_bins, labels=group_names)

# Group by age column
age_data = purchase_data.groupby(["Age"])
age_data.count()

# Total Count by age group
age_u10 = len(purchase_data.loc[purchase_data["Age"]=="<10", :])
age_10_14 = len(purchase_data.loc[purchase_data["Age"]=="10-14", :])
age_15_19 = len(purchase_data.loc[purchase_data["Age"]=="15-19", :])
age_20_24 = len(purchase_data.loc[purchase_data["Age"]=="20-24", :])
age_25_29 = len(purchase_data.loc[purchase_data["Age"]=="25-29", :])
age_30_34 = len(purchase_data.loc[purchase_data["Age"]=="30-34", :])
age_35_39 = len(purchase_data.loc[purchase_data["Age"]=="35-39", :])
age_40 = len(purchase_data.loc[purchase_data["Age"]=="40+", :])

# Create variable for total
total = age_u10 + age_10_14 + age_15_19 + age_20_24 + age_25_29 + age_30_34 + age_35_39 + age_40

# Percent of Players by age group
percent_u10 = (age_u10/total) * 100
percent_10_14 = (age_10_14/total) * 100
percent_15_19 = (age_15_19/total) * 100
percent_20_24 = (age_20_24/total) * 100
percent_25_29 = (age_25_29/total) * 100
percent_30_34 = (age_30_34/total) * 100
percent_35_39 = (age_35_39/total) * 100
percent_age_40 = (age_40/total) * 100

# Create dataframe
age_data_df = pd.DataFrame(
[[percent_u10, age_u10],
[percent_10_14, age_10_14],
[percent_15_19, age_15_19],
[percent_20_24, age_20_24],
[percent_25_29, age_25_29],
[percent_30_34, age_30_34],
[percent_35_39, age_35_39],
[percent_age_40, age_40]],
index=["<10","10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"],
columns=["Percentage of Players", "Total Count"])

# Format dataframe columns
age_data_df["Percentage of Players"] = age_data_df["Percentage of Players"].map("{:.2f}%".format)
age_data_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>2.95%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3.59%</td>
      <td>28</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.44%</td>
      <td>136</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>46.79%</td>
      <td>365</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>12.95%</td>
      <td>101</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>9.36%</td>
      <td>73</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.26%</td>
      <td>41</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.67%</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
# Bins and groupby from previous code block

age_data.count()

# purchase COUNT calculated in previous code block -- variables are: "age_u10", "age_15_19",
# "age_20_24", "age_25_29", "age_30_34", "age_35_39", "age_40" and used below.

# Average Purchase Price
u10_avg_price = purchase_data.loc[purchase_data["Age"]=="<10", "Price"].mean()
age1014_avg_price = purchase_data.loc[purchase_data["Age"]=="10-14", "Price"].mean()
age1519_avg_price = purchase_data.loc[purchase_data["Age"]=="15-19", "Price"].mean()
age2024_avg_price = purchase_data.loc[purchase_data["Age"]=="20-24", "Price"].mean()
age2529_avg_price = purchase_data.loc[purchase_data["Age"]=="25-29", "Price"].mean()
age3034_avg_price = purchase_data.loc[purchase_data["Age"]=="30-34", "Price"].mean()
age3539_avg_price = purchase_data.loc[purchase_data["Age"]=="35-39", "Price"].mean()
age40_avg_price = purchase_data.loc[purchase_data["Age"]=="40+", "Price"].mean()

# Total Purchase Value
u10_tot_val = purchase_data.loc[purchase_data["Age"]=="<10", "Price"].sum()
# print(u10_tot_val)
age1014_tot_val = purchase_data.loc[purchase_data["Age"]=="10-14", "Price"].sum()
# print(age1014_tot_val)
age1519_tot_val = purchase_data.loc[purchase_data["Age"]=="15-19", "Price"].sum()
# print(age1519_tot_val)
age2024_tot_val = purchase_data.loc[purchase_data["Age"]=="20-24", "Price"].sum()
# print(age2024_tot_val)
age2529_tot_val = purchase_data.loc[purchase_data["Age"]=="25-29", "Price"].sum()
# print(age2529_tot_val)
age3034_tot_val = purchase_data.loc[purchase_data["Age"]=="30-34", "Price"].sum()
# print(age3034_tot_val)
age3539_tot_val = purchase_data.loc[purchase_data["Age"]=="35-39", "Price"].sum()
# print(age3539_tot_val)
age40_tot_val = purchase_data.loc[purchase_data["Age"]=="40+", "Price"].sum()
# print(age40_tot_val)

# Average Purchase Total Per
u10_avg_tot_per = u10_tot_val / age_u10
# print(u10_avg_tot_per)
age1014_avg_tot_per = age1014_tot_val / age_10_14
# print(male_avg_tot_per)
age1519_avg_tot_per = age1519_tot_val / age_15_19
# print(age1519_avg_tot_per)
age2024_avg_tot_per = age2024_tot_val / age_20_24
# print(age2024_avg_tot_per)
age2529_avg_tot_per = age2529_tot_val / age_25_29
# print(age2529_avg_tot_per)
age3034_avg_tot_per = age3034_tot_val / age_30_34
# print(age3034_avg_tot_per)
age3539_avg_tot_per = age3539_tot_val / age_35_39
# print(age3539_avg_tot_per)
age40_avg_tot_per = age40_tot_val / age_40
# print(age40_avg_tot_per)

# Create dataframe
per_age_data_df = pd.DataFrame(
[[age_u10, u10_avg_price, u10_tot_val, u10_avg_tot_per],
[age_10_14, age1014_avg_price, age1014_tot_val, age1014_avg_tot_per],
[age_15_19, age1519_avg_price, age1519_tot_val, age1519_avg_tot_per],
[age_20_24, age2024_avg_price, age2024_tot_val, age2024_avg_tot_per],
[age_25_29, age2529_avg_price, age2529_tot_val, age2529_avg_tot_per],
[age_30_34, age3034_avg_price, age3034_tot_val, age3034_avg_tot_per],
[age_35_39, age3539_avg_price, age3539_tot_val, age3539_avg_tot_per],
[age_40, age40_avg_price, age40_tot_val, age40_avg_tot_per]],
index=["<10","10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"],
columns=["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Avg Purchase Total Per Person"])

# Formate dataframe columns
per_age_data_df["Average Purchase Price"] = per_age_data_df["Average Purchase Price"].map("${:.2f}".format)
per_age_data_df["Total Purchase Value"] = per_age_data_df["Total Purchase Value"].map("${:.2f}".format)
per_age_data_df["Avg Purchase Total Per Person"] = per_age_data_df["Avg Purchase Total Per Person"].map("${:.2f}".format)

per_age_data_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Purchase Total Per Person</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$3.35</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$2.96</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.04</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1114.06</td>
      <td>$3.05</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$2.93</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$3.60</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$2.94</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
# Trying to use less code!

total_purchase = purchase_data.groupby("SN")["Price"].sum()
purchase_count = purchase_data.groupby("SN")["Price"].count()
purchase_avg = purchase_data.groupby("SN")["Price"].mean()

# Create DataFrame
spender_df = pd.DataFrame({"Purchase Count": purchase_count, "Average Purchase Price": purchase_avg, "Total Purchase Value": total_purchase})

# Sort by Total Purchase Value
spender_df.sort_values("Total Purchase Value", ascending = False, inplace=True)

# Format columns
spender_df["Average Purchase Price"] = spender_df["Average Purchase Price"].map("${:.2f}".format)
spender_df["Total Purchase Value"] = spender_df["Total Purchase Value"].map("${:.2f}".format)

# Show Top 5 only
spender_df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
pop_tot = purchase_data.groupby("Item Name")["Price"].sum()
pop_count = purchase_data.groupby("Item Name")["Price"].count()
# Used .mean on item price --- there was a variance on item "Final Critic" (max price = 4.88, min price = 4.19)
pop_price = purchase_data.groupby("Item Name")["Price"].mean()

# Create DataFrame
pop_df = pd.DataFrame({"Purchase Count": pop_count, "Avg Item Price": pop_price, "Total Purchase Value": pop_tot})

# Sort by Purchase Count 
pop_df.sort_values("Purchase Count", ascending = False, inplace=True)

# Format columns
pop_df["Avg Item Price"] = pop_df["Avg Item Price"].map("${:.2f}".format)
pop_df["Total Purchase Value"] = pop_df["Total Purchase Value"].map("${:.2f}".format)

# Show Top 5 only
pop_df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>Persuasion</th>
      <td>9</td>
      <td>$3.22</td>
      <td>$28.99</td>
    </tr>
    <tr>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
# Create new DataFrame name using variables from previous code block
prof_df = pd.DataFrame({"Purchase Count": pop_count, "Avg Item Price": pop_price, "Total Purchase Value": pop_tot})

# Sort by Total Purchase Value
prof_df.sort_values("Total Purchase Value", ascending = False, inplace=True)

# Format columns
prof_df["Avg Item Price"] = prof_df["Avg Item Price"].map("${:.2f}".format)
prof_df["Total Purchase Value"] = prof_df["Total Purchase Value"].map("${:.2f}".format)

# Show Top 5 only
prof_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


