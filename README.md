# Dependencies and Setup
import pandas as pd
# File to Load (Remember to Change These)
file_to_load = "purchase_data.csv"
# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)
## Player Count
# Display the total number of players
total_players = len(purchase_data['SN'].unique())
total_players_df = pd.DataFrame({'Total Players': [total_players]})
## Purchasing Analysis (Total)
# Run basic calculations to obtain number of unique items, average price, etc.
# Create a summary data frame to hold the results
# Optional: give the displayed data cleaner formatting
# Display the summary data frame
num_unique_items = len(purchase_data['Item ID'].unique())
avg_price = purchase_data['Price'].mean()
num_purchases = purchase_data['Item ID'].count()
total_revenue = purchase_data['Price'].sum()
purchasing_analysis = pd.DataFrame({'Number of Unique Items': [num_unique_items], 'Average Price': [avg_price],'Number of Purchases': num_purchases,'Total Revenue': total_revenue})
purchasing_analysis['Average Price'] = purchasing_analysis['Average Price'].map("${:.2f}".format)
purchasing_analysis['Total Revenue'] = purchasing_analysis['Total Revenue'].map("${:,.2f}".format)
purchasing_analysis
## Gender Demographics
# Percentage and Count of Male Players
# Percentage and Count of Female Players
# Percentage and Count of Other / Non-Disclosed
unique_player_names = purchase_data.drop_duplicates(subset='SN', keep = 'first', inplace=False)[['SN', 'Age','Gender']]
demo_count = unique_player_names['Gender'].value_counts()
demo_percent = (demo_count/total_players * 100)
gender_demo = pd.DataFrame({'Total Count': demo_count, 'Percentage of Players':demo_percent})
gender_demo['Percentage of Players'] = gender_demo['Percentage of Players'].map('{:.2f}%'.format)
gender_demo
## Purchasing Analysis (Gender)
# Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender
# Create a summary data frame to hold the results
# Optional: give the displayed data cleaner formatting
# Display the summary data frame
grouped_gender = purchase_data.groupby(['Gender'])
purchase_count = grouped_gender['Item ID'].count()
avg_purchase_price = grouped_gender['Price'].mean()
total_purchase = grouped_gender['Price'].sum()
avg_total_purchase = total_purchase/ demo_count
purchasing_analysis = pd.DataFrame({'Purchase Count': purchase_count,
 'Average Purchase Price': avg_purchase_price,
 'Total Purchase Value': total_purchase,
 'Avg Total Purchase per Person': avg_total_purchase
 })
purchasing_analysis['Average Purchase Price'] = purchasing_analysis['Average Purchase Price'].map('${:.2f}'.format)
purchasing_analysis['Total Purchase Value'] = purchasing_analysis['Total Purchase Value'].map('${:.2f}'.format)
purchasing_analysis['Avg Total Purchase per Person'] = purchasing_analysis['Avg Total Purchase per Person'].map(
 '${:.2f}'.format)
purchasing_analysis
## Age Demographics
# Establish bins for ages
# Categorize the existing players using the age bins. Hint: use pd.cut()
# Calculate the numbers and percentages by age group
# Create a summary data frame to hold the results
# Optional: round the percentage column to two decimal points
# Display Age Demographics Table
bins = [0, 9, 14, 19, 24, 29, 34, 39, 50]
group_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']
unique_player_names['Age Range'] = pd.cut(unique_player_names['Age'], bins, labels= group_names)
grouped_age = unique_player_names.groupby(['Age Range'])
age_count = grouped_age['SN'].count()
age_percent = (age_count/ total_players * 100).map('{:.2f}%'.format)
age_demo = pd.DataFrame({'Total Count': age_count,
 'Percentage of Players': age_percent})
age_demo
## Purchasing Analysis (Age)
# Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below
# Create a summary data frame to hold the results
# Optional: give the displayed data cleaner formatting
# Display the summary data frame
purchase_data['Age Range'] = pd.cut(purchase_data['Age'], bins, labels = group_names)
grouped_age_purchase = purchase_data.groupby('Age Range')
purchase_count_age = grouped_age_purchase['Item ID'].count()
avg_purchase_price_age = grouped_age_purchase['Price'].mean()
total_purchase_age = grouped_age_purchase['Price'].sum()
avg_total_purchase_age = total_purchase_age / age_count
purchasing_analysis_age = pd.DataFrame({'Purchase Count': purchase_count_age,
 'Average Purchase Price': avg_purchase_price_age,
 'Total Purchase Value': total_purchase_age,
 'Avg Total Purchase per Person': avg_total_purchase_age
 })
purchasing_analysis_age['Average Purchase Price'] = purchasing_analysis_age['Average Purchase Price'].map(
 '${:.2f}'.format)
purchasing_analysis_age['Total Purchase Value'] = purchasing_analysis_age['Total Purchase Value'].map(
 '${:.2f}'.format)
purchasing_analysis_age['Avg Total Purchase per Person'] = purchasing_analysis_age['Avg Total Purchase per Person'].map('${:.2f}'.format)
purchasing_analysis_age
## Top Spenders
# Run basic calculations to obtain the results in the table below
# Create a summary data frame to hold the results
# Sort the total purchase value column in descending order
# Optional: give the displayed data cleaner formatting
# Display a preview of the summary data frame
grouped_spenders = purchase_data.groupby('SN')
purchase_count = grouped_spenders['Item ID'].count()
avg_price = grouped_spenders['Price'].mean()
total_price = grouped_spenders['Price'].sum()
top_spenders = pd.DataFrame({'Purchase Count': purchase_count,
 'Average Purchase Price': avg_price,
 'Total Purchase Value': total_price
 })
top_spenders = top_spenders.sort_values('Total Purchase Value', ascending = False)
top_spenders['Average Purchase Price'] = top_spenders['Average Purchase Price'].map('${:.2f}'.format)
top_spenders['Total Purchase Value'] = top_spenders['Total Purchase Value'].map('${:.2f}'.format)
top_spenders.head()
## Most Popular Items
# Retrieve the Item ID, Item Name, and Item Price columns
# Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value
# Create a summary data frame to hold the results
# Sort the purchase count column in descending order
# Optional: give the displayed data cleaner formatting
# Display a preview of the summary data frame
popular_items = purchase_data[['Item ID', 'Item Name', 'Price']]
grouped_item = popular_items.groupby(['Item ID', 'Item Name'])
purchase_count = grouped_item['Price'].count()
item_price = grouped_item['Price'].max()
total_value = purchase_count * item_price
most_popular = pd.DataFrame({'Purchase Count': purchase_count,
 'Item Price': item_price,
 'Total Purchase Value': total_value
 })
most_popular_purchase = most_popular.sort_values('Purchase Count', ascending = False)
most_popular_purchase['Item Price'] = most_popular_purchase['Item Price'].map('${:.2f}'.format)
most_popular_purchase['Total Purchase Value'] = most_popular_purchase['Total Purchase Value'].map('${:.2f}'.format)
most_popular_purchase.head()
## Most Profitable Items
# Sort the above table by total purchase value in descending order
# Optional: give the displayed data cleaner formatting
# Display a preview of the data frame
most_profitable = most_popular.sort_values('Total Purchase Value', ascending = False)
most_profitable['Item Price'] = most_profitable['Item Price'].map('${:.2f}'.format)
most_profitable['Total Purchase Value'] = most_profitable['Total Purchase Value'].map('${:.2f}'.format)
most_profitable.head()
