# Import necessary packages
import pandas as pd
import numpy as np

# Begin coding here ...
# Use as many cells as you like

df_price = pd.read_csv('data/airbnb_price.csv')
df_roomtype = pd.read_excel('data/airbnb_room_type.xlsx')
df_review = pd.read_csv('data/airbnb_last_review.tsv',sep='\t')

#merge dfs:
merged_df = pd.merge(df_price,df_roomtype,on='listing_id')
df = pd.merge(merged_df,df_review,on='listing_id')

#The first and most recent reviews:
first_reviewed = pd.to_datetime(df['last_review']).min()
print('The first review:',first_reviewed)
last_reviewed = pd.to_datetime(df['last_review']).max()
print('The most recent review:',last_reviewed)

#number of private rooms:

df['room_type']= df['room_type'].str.lower()
nb_private_rooms = df[df['room_type'] == 'private room'].shape[0]
print('The number of private rooms is:', nb_private_rooms)

#Next I calcualted the average listing price:
#first I cleaned the price column:
df['price'] = df['price'].str.replace('dollars','').astype(float)
avg_price = df['price'].mean().round(2)
print('The average listing price is:',avg_price)

#create a new df with all the new variables created:
data = {'first_reviewed':[first_reviewed],'last_reviewed':[last_reviewed] ,'nb_private_rooms':[nb_private_rooms],'avg_price':[avg_price]}
review_dates = pd.DataFrame(data)
print(review_dates)


The first review: 2019-01-01 00:00:00
The most recent review: 2019-07-09 00:00:00
The number of private rooms is: 11356
The average listing price is: 141.78
  first_reviewed last_reviewed  nb_private_rooms  avg_price
0     2019-01-01    2019-07-09             11356     141.78
