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
