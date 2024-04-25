# NYC Green Taxi Trip Exploration

## Project Synopsis

This investigation delves into the NYC Green Taxi Trip Data spanning from January 2022 to January 2023. Green Taxis, distinguished by their authorization for street-hail pickups primarily outside the bustling central business district and airports, were initiated to broaden taxi accessibility to lesser-served regions outside of Manhattan. Historically, Yellow Taxis were the primary operators within Manhattan and at airports, enjoying exclusive access to high-demand locales. However, with Green Taxis now entering these territories, there's been a notable expansion in service reach. The essence of this study is to apply machine learning methodologies for both regression and classification purposes, focusing on two principal objectives:

1. ***Predicting Fare Amounts***: By leveraging key features such as pickup and drop-off coordinates, journey length, DateTime, among other relevant factors, the aim is to construct a regression model. This model will endeavor to accurately predict individual taxi trip fares, refining fare estimation processes.
2. ***Identifying Profitable Pickup Points***: Through analysis of pickup locales, travel distances, and fare data, the ambition is to develop a clustering model. This model will pinpoint locations where taxi pickups could prove most lucrative, aiding drivers in optimizing their positioning for maximized earnings.

## Dataset Characteristics

1. ***Trip Record***: Sourced from the New York City Taxi and Limousine Commission (TLC), comprising 908,613 trip entries.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/1.png)


2. **Taxi Zone Mapping**: Facilitates the correlation of location IDs from the primary dataset with boroughs, zones, and service areas within NYC, encompassing 265 records.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/2.png)


3. **Holiday Insights**: A specially curated dataset to investigate trip dynamics across holidays, regular workdays, and weekends, containing data on 23 holidays.

## Initial Data Distribution Analysis

### 1. Trip Distance
Analysis of trip distances across 908,613 records highlights an average journey length of 78.72 miles, with a maximum reaching up to 360,068.14 miles. A closer look shows most trips ranging between 1.15 and 3.73 miles, the median distance being 2.00 miles.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/3.png)


### 2. Fare Metrics
The average fare stands at $15.39, with fares fluctuating between -$350.08 and $2020.20. A significant portion of trips cost between $7.90 and $18.20, with the median fare at $11.50.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/4.png)


### 3. Payment Trends

Credit Card payments dominate, representing 64% of transactions, followed by Cash at 36%. Less than 1% of trips experienced non-payment or disputes.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/5.png)



## Feature Engineering
### 1. Integrating Taxi Zones
Merging Taxi Zone data with the main trip dataset enriches it with additional geographic insights.

### 2. Data Streamlining
- We are dropping columns deemed unnecessary for our analysis:
  - ***ehail_fee***: Contains zero records.
  - ***passenger_count***: Prone to inaccuracies as it relies on driver input.
  - ***store_and_fwd_flag, trip_type, payment_type***: Irrelevant for price prediction.
- We are also dropping columns where there is not a single entry.
  
### 3. Renaming for Clarity
Columns like 'lpep_pickup_datetime' are renamed for consistency and readability.

### 4. Refining Trip Data
We filter trips based on various criteria:

- ***Fare Amount***: Removing trips below the base fare of $3.
- ***Trip Distance***: Filtering out trips with negative or excessive (over 100 miles) distances.
- ***Time Constraints***: Filtering out trips with negative or excessive (over 180 minutes) time.

### 5. Handling Missing Values
Trips with any missing data across columns are dropped.

### 6. DateTime Feature Extraction
We extract additional DateTime features into new columns:

- Features like 'dayofweek', 'pickuphour', 'drophour', 'pickupdate', 'dropoffdate' are created.
- A new column identifies trips conducted during peak hours (7 am to 10 am and 5 pm to 7 pm).
- Marking trips conducted on holidays for further analysis.

### 7. Congestion Surcharge Flagging
Incorporating a flag to denote the presence of a congestion surcharge in the trip data.

## Exploratory Data Analysis

### 1. Vendor
In New York, taxi services are offered by two vendors: Creative Mobile Technologies, LLC (Vendor 1) and VeriFone Inc. (Vendor 2). The data distribution indicates a substantial disparity in trip counts between the vendors, with Vendor 2 accounting for 66.7% of total trips. In comparison, Vendor 1 has a notably lower contribution with 33.3% of total trips.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/6.png)

### 2. Rate Code
Six distinct rate codes are employed at the end of taxi trips, with the Standard rate being the most common, accounting for 71,000 trips, followed by the Negotiated rate code with 22,179 trips. The JFK rate code was utilized in 2,500 trips, whereas the rate codes for "Newark and Nassau" or Westchester were the least utilized.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/7.png)

### 3. Hourly Distribution of Trip Counts
The demand for taxi trips in New York City follows a distinctive pattern over a 24-hour cycle. Beginning with the morning rush around 6:00 AM, demand gradually increases as commuters begin their day, steadily rising through the late morning. Peak demand occurs in the late afternoon, typically between 4:00 PM and 6:00 PM, aligning with rush hour and the end of the workday, indicating a surge in transportation needs. Following this peak, demand decreases in the evening, gradually declining after peak hours and notably diminishing during the late-night period, reaching its lowest point around 4:00 AM to 5:00 AM. This cyclical trend illustrates fluctuations in demand, reflecting the daily rhythms and commuter patterns of the city.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/8.png)

### 4. Trip Distance

After filtering out trips with negative distances or excessive ones over 100 miles during the data cleaning process, the dataset contained 740,737 entries. The average trip distance noticeably decreased to 2.99 miles. Trip distances now range from 0.01 miles to a maximum of 95.50 miles, with the majority of data lying between 1.28 and 3.60 miles. The median distance is 2.03 miles.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/9.png)

### 5. Fare Amount

After eliminating trips below the base fare of $3, the average trip cost decreased to $13.82, with a reduced standard deviation of $11.31. The cost range now spans from $3.50 to $499.00, with the majority of trips occurring within the $7.50 to $16.00 range. The median cost after cleaning is $10.50.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/10.png)

### 6. Trip Time

Following the exclusion of trips with negative or excessively long durations (over 180 minutes), the dataset underwent cleaning, resulting in 740,737 entries. The average trip duration decreased to approximately 14.95 minutes, with a reduced standard deviation of 11.87 minutes. Trip durations now range from 1.02 minutes to a maximum of 178.55 minutes. The majority of trips fall within the range of 7.75 to 18.32 minutes, with the median duration standing at 11.97 minutes. This cleaning process ensured a more accurate representation of trip durations within a reasonable timeframe for analysis.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/11.png)

### 7. Trip distribution by Day

The demand for cabs exhibits a fluctuating pattern throughout the week, spanning from Monday (0) to Sunday (6). Wednesday (3) stands out as the peak demand day, recording 117,543 trips, closely followed by Tuesday (2) and Thursday (4) with 114,668 and 114,479 trips, respectively. Monday (0) shows a slightly lower demand at 103,797 trips, indicating a moderate start to the week. However, the demand gradually declines towards the weekend, notably dropping on Saturday (5) and Sunday (6) to 96,623 and 82,432 trips, respectively. This trend suggests a higher demand during the midweek, gradually tapering off towards the weekend.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/12.png)

### 8. Number of Trips by Pickup and Dropoff LocationID

- Pickup
  ![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/13.png)

- Dropoff
  ![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/14.png)

The analysis of cab demand unveils distinct patterns across various neighborhoods in New York City. East Harlem North and South emerge as top pickup locations, experiencing high demand due to their residential settings and cultural attractions. Similarly, Central Harlem, Morningside Heights, and Forest Hills also show significant pickup interest. Concerning dropoff locations, East Harlem North and South, Central Harlem, and Upper East Side North are prominent, indicating frequent trip completions in these areas. These neighborhoods feature a blend of residential, commercial, and cultural elements, attracting diverse trip demands likely influenced by residential density, cultural landmarks, and commercial activity. Located east and west of Central Park, the Upper East and Upper West Sides offer affluent residential areas, while Morningside Heights boasts a university-centric landscape. Overall, the concentrated demand in specific neighborhoods underscores their unique characteristics, shaping taxi usage trends in these distinct locales.

### 9. Distance VS Fare Amount

The fare amount typically aligns with the distance covered during a trip, exhibiting a general pattern of increase. However, upon closer examination of the dataset, anomalies become apparent, particularly instances where the fare amount significantly surpasses the expected cost for a given distance. There were numerous trips where the fare amount spiked to as high as $500, despite the distance being relatively short, approximately 5-6 miles. These anomalies suggest potential irregularities or factors beyond distance that influence fare calculations, necessitating further investigation into the specific conditions or variables contributing to these unusually high fares for comparatively short distances.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/15.png)

### 10. Distance VS Pickup Hour

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/16.png)

The data predominantly clusters within trip distances ranging from 1.28 to 3.60 miles. Interestingly, the average trip distance throughout the day consistently hovers around 3 to 3.5 miles. Notably, there's a steady trend of trip distances increasing from the early morning until the evening. However, during the early and late night hours, there's a noticeable uptick in the distance covered by taxi trips. This shift suggests evolving travel patterns or preferences across different times of the day, potentially indicating longer rides or varying travel behaviors among passengers during these hours.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/16.png)

### 11. Fare Amount VS Week Day

Over the course of the week, the fare range demonstrates a steady increase, reaching its highest point on Saturday at $14.20. This upward trend in fares could be attributed to heightened demand, shifts in travel behavior, or an influx of longer trips, particularly on Saturdays.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/17.png)

### 12. Tip Amount VS Hours

A significant increase in tips occurs around the 5th hour, followed by a decline, stabilizing between $1.50 and $2.00 until approximately the 15th hour. Subsequently, there's a gradual uptick in tip amounts, peaking around the 20th hour. These fluctuations underscore diverse tipping patterns associated with the pickup hour.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/18.png)

### 13. Tip Amount VS Week Day

Tips display interesting variations throughout the week, showing a gradual increase from Monday (day 0) to Wednesday (day 2), followed by fluctuations between Wednesday and Friday. Notably, there's a significant surge in tip amounts from Friday to Sunday, indicating a substantial rise towards the end of the week. This trend suggests an escalating pattern of tips as the week advances, with higher tipping tendencies observed over the weekend.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/6a352ec85e810156c9180aa5f22b070a2a065b58/Assests/19.png)


## Splitting Data & Dimension Reduction

During the data preparation phase for regression analysis, we assigned the 'fare_amount' as the target variable 'y', while the remaining columns were considered as features 'X'. Categorical variables such as 'VendorID', 'peak_hours', 'holiday_flag', 'pickup_dayofweek', and 'pickup_hour' were encoded to facilitate model training. Afterwards, the dataset was divided into training and testing sets using a 60-40 split for evaluation purposes. Furthermore, we employed dimensionality reduction techniques on the independent variables, where the first three components were able to capture a substantial amount of the dataset's variance.

![image](https://github.com/BhaveshxPurohit/Analyzing-NYC-Green-Taxi-Trips/blob/31a170312beae79c5898571a4e83675413b15790/Assests/21.png)


## Modeling

Here's a summary of the regression models implemented to predict fare amounts:

- **KNN Regression**: Yielded a Mean Squared Error of 10.2805 and a strong R-squared value of 0.9194, suggesting a good level of accuracy in predicting the target variable.

- **Decision Tree**: Exhibited a Mean Squared Error of 11.5379 and a robust R-squared value of 0.9096, indicating a reasonable fit to the data with a relatively low level of error.

- **Linear Regression**: Produced a Mean Squared Error of 13.8509 and an R-squared value of 0.8915, suggesting a moderate level of accuracy in predicting the target variable with a linear model.

- **Lasso Regression**: Achieved a remarkably low Mean Squared Error of 0.1036 and an impressive R-squared value of 0.9992, indicating a highly accurate fit to the data.

Among the four models tested, Lasso Regression emerged as the top performer, boasting an exceptionally low Mean Squared Error and impressive R-squared value, indicative of its highly accurate fit to the data. Following closely behind, KNN Regression demonstrated a respectable balance between simplicity and accuracy. Meanwhile, Decision Tree Regression exhibited a reasonable fit to the data. Finally, Linear Regression, while performing adequately, showed a slightly higher MSE and R-squared value, suggesting a moderate level of accuracy with its linear model. Overall, Lasso Regression proved to be the most promising model for predicting the target variable, followed by KNN Regression, Decision Tree Regression, and Linear Regression, in respective order of performance.



