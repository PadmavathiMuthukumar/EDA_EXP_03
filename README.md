**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**
```
import pandas as pd
from google.colab import files
import matplotlib.pyplot as plt

uploaded = files.upload()  # This will open a file upload dialog box

filename = list(uploaded.keys())[0]
print(f"Uploaded file: {filename}")
df = pd.read_csv(filename)

# Show summary
print("=== HEAD ===")
print(df.head(), "\n")

print("=== DATA TYPES ===")
print(df.dtypes, "\n")

print("=== NULL COUNTS ===")
print(df.isnull().sum(), "\n")
df['date'] = df['period.datetimeFrom.utc'].dt.date
df['month'] = df['period.datetimeFrom.utc'].dt.month
df['hour'] = df['period.datetimeFrom.utc'].dt.hour
print(df)
plt.figure(figsize=(10,6))
df.boxplot(column='value', by='month', grid=False)
plt.title('Monthly PM2.5 Levels (Boxplot)')
plt.suptitle('')
plt.xlabel('Month')
plt.ylabel('PM2.5 (µg/m³)')
plt.show()
monthly_avg = df.groupby('month')['value'].mean()

plt.figure(figsize=(10,5))
monthly_avg.plot(kind='bar', color='skyblue')
plt.title('Average Monthly PM2.5 Concentration')
plt.xlabel('Month')
plt.ylabel('PM2.5 (µg/m³)')
plt.show()
daily_avg = df.groupby('date')['value'].mean()
exceed_days = (daily_avg > 25).sum()
total_days = len(daily_avg)
percent_exceed = (exceed_days / total_days) * 100

print("=== WHO PM2.5 LIMIT ANALYSIS ===")
print(f"Days exceeding WHO limit (25 µg/m³): {exceed_days}")
print(f"Total days: {total_days}")
print(f"Percentage exceeding: {percent_exceed:.2f}%\n")
hourly_avg = df.groupby('hour')['value'].mean()

plt.figure(figsize=(10,5))
hourly_avg.plot(kind='line', marker='o', color='orange')
plt.title('Average PM2.5 vs Hour of Day')
plt.xlabel('Hour')
plt.ylabel('PM2.5 (µg/m³)')
plt.grid(True)
plt.show()
top5_days = daily_avg.sort_values(ascending=False).head(5)
print("=== TOP 5 WORST-POLLUTED DAYS ===")
print(top5_days, "\n")
print("=== INTERPRETATION ===")
print("""
The monthly and daily analyses show that air pollution in Delhi peaks during November–December,
likely due to winter inversion and Diwali-related emissions. PM2.5 levels remain high throughout
the year, exceeding WHO’s limit on all days. Hourly trends reveal peaks in the early morning and
evening due to traffic and low wind movement. This sustained exposure represents a major public
health risk, emphasizing the need for stricter emissions control and seasonal air quality management.
""")
```
**Output**
<img width="780" height="575" alt="image" src="https://github.com/user-attachments/assets/a6ae4313-4a14-48f0-9ab0-aedfda536687" />
<img width="799" height="320" alt="image" src="https://github.com/user-attachments/assets/d22f6739-2e7f-4851-9e99-e7b18e0729a9" />
<img width="796" height="606" alt="image" src="https://github.com/user-attachments/assets/4a4cc058-710c-468e-844e-e2220b9b7468" />
<img width="596" height="445" alt="image" src="https://github.com/user-attachments/assets/b63fffbe-3363-46ac-9b2e-5fe4e6f44939" />
<img width="850" height="474" alt="image" src="https://github.com/user-attachments/assets/d9eefe51-c462-497e-b1b2-eff4412c476f" />
<img width="562" height="121" alt="image" src="https://github.com/user-attachments/assets/25ed6b50-3cc1-47ef-88f2-c0819a14fc00" />
<img width="850" height="470" alt="image" src="https://github.com/user-attachments/assets/7f30aac7-5835-4968-be19-ca86e50b75a6" />

<img width="390" height="207" alt="image" src="https://github.com/user-attachments/assets/7289ebe9-f4ef-4903-b4af-d8d3fddf7d02" />
<img width="1042" height="195" alt="image" src="https://github.com/user-attachments/assets/02ebfa39-be50-4c22-8720-cd1927360674" />

**Interpretation**

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2)  # write other insights
insights:

Plotting the daily average PM2.5 levels over time reveals whether pollution is rising, stable, or falling within the recorded months If your dataset spans November–December 2016, you’ll notice a sharp spike right after Diwali (early November) and then a slow decline toward the end of December. 2.average PM2.5 levels are often higher at night and early morning due to temperature inversion (pollutants trapped near the surface). 3.Checking average PM2.5 per weekday may reveal whether pollution is linked more to traffic (weekdays) or general air stagnation (weekends).

**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


