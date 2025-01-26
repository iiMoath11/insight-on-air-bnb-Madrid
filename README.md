# insight-on-air-bnb-Madrid
Analyzing the growth of Airbnb listings, seasonal demand, and popular neighborhoods in Madrid.
## For the following analysis i have downloaded the data from https://data.insideairbnb.com/spain/comunidad-de-madrid/madrid/2024-09-11/data/calendar.csv.gz

``` diff
import pandas as pd
data = pd.read_csv('calendar.csv')
```

# in this code we know hopw much avalaibe and un avaliable data
``` diff
data.available.value_counts()
```
<img width="589" alt="Screenshot 2025-01-22 at 11 26 54 AM" src="https://github.com/user-attachments/assets/34908f89-0e72-4ae2-9140-a02bd37c44a1" />

# We calculate the percentage of avaliable and unavaliable date
``` diff
avi_per = data['available'].value_counts(normalize = True)*100
avi_per
```
<img width="589" alt="Screenshot 2025-01-22 at 11 27 13 AM" src="https://github.com/user-attachments/assets/cd2a3a76-53f4-41f5-828e-3d502b090853" />

# Counts for the busiest day!
``` diff
bus_day = data[data['available']== 'f']['date'].value_counts()
bus_day.head()
```
<img width="602" alt="Screenshot 2025-01-22 at 11 31 47 AM" src="https://github.com/user-attachments/assets/2e01a714-0a74-422a-bdcb-2893a9e34691" />

# Plot a bar graph to show availability percentage
``` diff
import matplotlib.pyplot as plt
avi_per.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel( 'Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="863" alt="Screenshot 2025-01-26 at 12 39 39 PM" src="https://github.com/user-attachments/assets/64fd1cc6-b896-4876-bf25-b6ee049d4164" />

# Plot the busiest day
``` diff
bus_day.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="863" alt="Screenshot 2025-01-26 at 12 40 59 PM" src="https://github.com/user-attachments/assets/7ea793fd-6258-421b-88b6-0f1f7b45e1f7" />

# 📄 Download listings dataset of Madrid from
https://data.insideairbnb.com/spain/comunidad-de-madrid/madrid/2024-09-11/data/listings.csv.gz
``` diff
import pandas as pd
listings = pd.read_csv('listings.csv')

listings.columns
```
<img width="863" alt="Screenshot 2025-01-26 at 12 44 12 PM" src="https://github.com/user-attachments/assets/bcc60c34-5848-496e-b028-79866b261e3f" />

# ✅ Room type and price is given seperately
Let`s Combine and visualize them
``` diff
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type_encoded')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='purple')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
<img width="863" alt="Screenshot 2025-01-26 at 2 18 18 PM" src="https://github.com/user-attachments/assets/943180ed-5ed2-4999-9b0c-21ba06cac2a8" />
