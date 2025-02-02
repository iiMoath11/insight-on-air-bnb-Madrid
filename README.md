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

# ✅ Which are the top 10 neighborhoods with the most listings?
``` diff
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img width="530" alt="Screenshot 2025-02-02 at 2 15 16 PM" src="https://github.com/user-attachments/assets/9bff0546-83c8-4fff-8ad8-066384f2b6dc" />

# ✅ Geographical Distribution of Listings (Price Colored)
``` diff
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="716" alt="Screenshot 2025-02-02 at 2 16 18 PM" src="https://github.com/user-attachments/assets/019edd9d-ed7d-4648-ae34-40345d0c6718" />

# Let us see the listings on a real map
- Hotter Areas (Red/Yellow):
1- High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. These areas are likely to have high demand.
2- Popular Locations: These regions might be more popular or in high demand. They could be near major landmarks, popular neighborhoods, or central areas of Madrid where people tend to stay more often. Examples could include areas near Gran Via, Puerta del Sol, or Retiro Park, where tourists and locals frequent.

- Colder Areas (Green/Blue):
1- Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available and thus are less competitive.
2- Less Popular Locations: These areas might be less popular or farther from key attractions. In Madrid, this could include areas outside the city center, such as suburban districts or neighborhoods further from the major tourist spots. If you’re analyzing pricing, lower density could imply that these areas are more affordable, as there may be less tourist traffic or demand.

- Density Patterns:
1- Clustered Areas: If you notice clusters of heatmap intensity in certain locations (more red or yellow), these represent hotspots. These areas are high-traffic and likely to be popular with tourists or locals. In Madrid, these could be areas like Malasaña, Sol, or Chamberí, where there’s a higher volume of accommodation listings due to cultural, commercial, or nightlife activity.
2- Spread-Out Listings: If the heatmap shows a more uniform distribution of color (milder heatmap tones), it might suggest that listings are evenly spread across the region. This could indicate a more balanced demand for rentals across different parts of Madrid, such as residential areas on the outskirts or other less tourist-heavy districts, offering affordable and varied accommodation options.

``` diff
import folium
from folium.plugins import HeatMap
import pandas as pd

# Sample data for Madrid (latitude, longitude, price)
madrid_data = listings[['latitude', 'longitude', 'price']]  # Example, you may add more columns

# Create a base map centered around Madrid
m = folium.Map(location=[40.4168, -3.7038], zoom_start=12)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in madrid_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('madrid_heatmap.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```
<img width="1036" alt="Screenshot 2025-02-02 at 2 20 45 PM" src="https://github.com/user-attachments/assets/35a290fe-56fc-4d2e-821f-ca03b762322e" />

# 🚨 How do I find location for my city?

1- Type your city name on google maps
2- Click on What`s here

<img width="1137" alt="Screenshot 2025-02-02 at 2 23 48 PM" src="https://github.com/user-attachments/assets/91b196de-7553-44cc-8304-efa1ed8d11db" />
- You will find latitude and longitude at the bottom of screen
<img width="518" alt="Screenshot 2025-02-02 at 2 24 26 PM" src="https://github.com/user-attachments/assets/c8144b25-16c5-47f1-ac1f-b62671a86a44" />




