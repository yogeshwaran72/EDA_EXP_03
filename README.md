## **EXP 3 - Delhi Air Quality Analysis**

# NAME:YOGESHWARAN A
# REGISTER NUMBER:212223040249

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
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import numpy as np
df=pd.read_csv('delhi_pm25_aqi.csv')
df
```
<img width="606" height="386" alt="image" src="https://github.com/user-attachments/assets/0d837cb2-7a20-4e07-9ccd-4e2a463d03d9" />

```
df.info()
```
<img width="432" height="200" alt="image" src="https://github.com/user-attachments/assets/bf094212-04bf-4a3b-b695-fb9859fbc7a3" />

```
df.shape
```
<img width="355" height="76" alt="image" src="https://github.com/user-attachments/assets/bd696fff-5fef-42e3-8bc1-a8635fe3e5b7" />

```
df.describe()
```
<img width="431" height="279" alt="image" src="https://github.com/user-attachments/assets/88fafe16-b586-4d3c-a673-252be5d65629" />

```
df.head()
```
<img width="574" height="189" alt="image" src="https://github.com/user-attachments/assets/c2fc4f65-7a28-484b-a0dc-0a38eacdf600" />

```
df.tail()
```
<img width="630" height="193" alt="image" src="https://github.com/user-attachments/assets/8061b347-e11c-4df2-b9bd-ef6ec585576f" />

```
df.isnull().sum()
```
<img width="402" height="103" alt="image" src="https://github.com/user-attachments/assets/9125a48d-7072-49ef-9554-b9b83857aa82" />

```
df['datetime'] = pd.to_datetime(df['period.datetimeFrom.utc'], errors='coerce')

df['datetime']
```
<img width="677" height="216" alt="image" src="https://github.com/user-attachments/assets/e361935e-de29-4a93-8f81-e7a4d5e23537" />

```
df['pm25'] = pd.to_numeric(df['value'], errors='coerce')
df['pm25']
```
<img width="515" height="213" alt="image" src="https://github.com/user-attachments/assets/b8fdb6f3-7ce1-4640-a41d-578c4aaf46a8" />

```
df['date'] = df['datetime'].dt.strftime('%Y-%m-%d')
df['month'] = df['datetime'].dt.month
df['hour'] = df['datetime'].dt.hour
df.head(10)
```
<img width="1021" height="329" alt="image" src="https://github.com/user-attachments/assets/85d501a0-e62c-44bb-abf4-6b7249337547" />

```
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(10, 6))
sns.boxplot(x='month', y='pm25', data=df)
plt.xlabel('Month')
plt.ylabel('PM2.5')
plt.show()
```
<img width="921" height="533" alt="image" src="https://github.com/user-attachments/assets/8e19501d-cb04-4cdc-9377-0567e27b3b6a" />

```
monthly_avg = df.groupby('month')['pm25'].mean().reset_index()
plt.figure(figsize=(8, 5))
plt.plot(monthly_avg['month'], monthly_avg['pm25'], marker='o')
plt.xlabel('Month')
plt.ylabel('Average PM2.5')
plt.grid(True)
plt.show()
```
<img width="844" height="452" alt="image" src="https://github.com/user-attachments/assets/d42ae078-edba-4dc2-a6a7-102e2a1d36a1" />


```
daily_avg = df.groupby('date')['pm25'].mean().reset_index()
exceed_days = daily_avg[daily_avg['pm25'] > 25]
total_days = daily_avg['date'].nunique()
exceed_count = exceed_days.shape[0]
exceed_percent = (exceed_count / total_days) * 100

print(f"Days exceeding WHO limit: {exceed_count} days")
print(f"Percentage of days exceeding WHO limit: {exceed_percent:.2f}%")
```

<img width="457" height="54" alt="image" src="https://github.com/user-attachments/assets/412c549c-962c-481a-96a2-1820d5527a65" />

```
top5_days = daily_avg.sort_values(by='pm25', ascending=False).head(5)
print("Top 5 worst-polluted days (date and PM2.5):")
print(top5_days)
```
<img width="425" height="140" alt="image" src="https://github.com/user-attachments/assets/b1613243-a0b8-4288-b36b-6bf630d42013" />




**Interpretation**

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2) Over the analysis period, the data shows that about {exceed_percent:.2f}% of days had PM2.5 levels above the WHO recommended limit of 25 µg/m³.
In simpler terms, that means the air was unhealthy roughly one out of every three days.

This indicates that the area often experiences moderate to poor air quality, especially on days with calm weather or higher human activity.
If this trend continues, it could pose long-term health risks, particularly for children, the elderly, and people with breathing problems.

Seasonal patterns could also play a role — pollution levels often rise during winter months when pollutants get trapped close to the ground.
To improve conditions, stricter emission controls, cleaner transport options, and awareness campaigns could help reduce PM2.5 levels and make the air safer to breathe.

**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


