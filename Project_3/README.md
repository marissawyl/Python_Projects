# Introduction

This project continues my learning journey to strengthen my **Python** and **data analysis** skills. Building on insights from my previous exploration of the 2024 job market dataset, this project focuses on a new dataset: Uber trip data from New York City (2014–2015), originally shared by _Shan Singh_ on Udemy. Through this project, I practiced data wrangling, exploratory analysis, and visualization to uncover travel patterns and gain a deeper understanding of how to work with real-world datasets.

If you’re interested in seeing the earlier part of this Python learning series, where I analyzed job market trends in India, feel free to check out my [Project 2](https://github.com/marissawyl/Python_Projects/tree/main/Project_2).

# Background

Ride-hailing platforms like Uber generate a large amount of data every day. Analyzing such data can provide valuable information about passenger demand, peak hours, and how driver supply responds to that demand. In this project, I focused on understanding when and where demand is highest, identifying rush hour patterns across weekdays and months, and comparing the relationship between active vehicles and completed trips. Beyond being a technical practice, this project also demonstrates how Python can be applied to real-life scenarios.

# Questions to Analyze

These are the main questions explored in this project:

1. How does ride demand change over months and between weekdays and weekends?
2. What are the peak demand hours for Uber rides in NYC, and how do they vary across weekdays and weekends?
3. Does the relationship between time of day and month reveal shifting travel behaviors in New York City’s Uber demand?
4. Can the relationship between driver supply and trip demand reveal how efficiently Uber balanced the two across its New York City bases?

# Tools Used

To gain insights into the data analyst job market, I worked with a range of tools and technologies that supported every stage of the project:
- **Python**: Served as the foundation of my analysis, helping me process data and uncover key insights. To extend its capabilities, I incorporated several powerful libraries:
    - **Pandas**: Used for cleaning, transforming, and structuring the dataset efficiently.
    - **NumPy**: Used for numerical operations such as computing linear regression (slope and intercept) and ensuring consistent scaling across visualizations.
    - **Matplotlib**: Helped me build clear, data-driven visualizations.
    - **Seaborn**: Enhanced my charts with refined and professional visuals.
- **Jupyter Notebook**: Provided an interactive space to experiment with code, document findings, and combine visuals with analysis.
- **Visual Studio Code**: Offered a reliable and organized environment for writing and testing scripts.

# Data Preparation and Cleanup

Before beginning the analysis, I prepared each dataset to ensure it was ready for exploration and visualization. Depending on the data source, the preparation steps involved either loading a single CSV file or combining multiple files into one unified dataset.

### Accessing and Loading a Single File

For datasets available as a single CSV file, I simply accessed and loaded the data into a Pandas DataFrame for further processing.

```python
# Importing Libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from adjustText import adjust_text
import matplotlib.lines as mlines

# Loading Data
df = pd.read_csv(r'D:\Data Analyst stuffs\Python stuffs\Learn Python for Data Analysis\PYTHON_EXERCISES_UDEMY\Uber Case Study\Datasets\uber-raw-data-janjune-15_sample.csv')

# Data Cleanup
df['Pickup_date'] = pd.to_datetime(df['Pickup_date'])
df = df.drop_duplicates()
```

### Accessing and Combining Multiple Files

Some datasets were split across several CSV files. In those cases, I loaded each file and concatenated them into a single DataFrame to make the analysis more efficient and consistent.

```python
# Importing Libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import os

# === Accessing and Combining Multiple CSV Files ===

# List all dataset files in the folder
files = os.listdir(r'D:\Data Analyst stuffs\Python stuffs\Learn Python for Data Analysis\PYTHON_EXERCISES_UDEMY\Uber Case Study\Datasets')

# Select last 8 files and remove unnecessary ones
data_set = files[-8:]
files_to_remove = ['uber-raw-data-janjune-15.csv', 'uber-raw-data-janjune-15_sample.csv']
data_set = [f for f in data_set if f not in files_to_remove]

# Initialize an empty DataFrame to store all data
df_final = pd.DataFrame()
path = r'D:\Data Analyst stuffs\Python stuffs\Learn Python for Data Analysis\PYTHON_EXERCISES_UDEMY\Uber Case Study\Datasets'

# Load and combine all datasets into a single DataFrame
for file in data_set:
    current_df = pd.read_csv(path + '/' + file)
    df_final = pd.concat([current_df, df_final])

# Data Cleanup
df_final.drop_duplicates(inplace=True)
df_final['Date/Time'] = pd.to_datetime(df_final['Date/Time'])
```

# The Analysis

## 1. How does ride demand change over months and between weekdays and weekends?

To answer this, I compared ride demand across months and broke it down by day of the week. By visualizing monthly trends alongside weekday patterns, I could see both overall growth and the weekly distribution of activity. This helped me identify not only when demand peaked, but also which days consistently drove higher volumes.

View my notebook with detailed steps here: [1_Uber_Ride_Demand_Trends.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/1_Uber_Ride_Demand_Trends.ipynb)

### Visualize Data

```python
# Apply seaborn theme for cleaner visualization
sns.set_theme(style='ticks')

# Create bar plot: x = month, y = count, hue = weekday
ax = sns.catplot(
    data=df_pickup_grouped,
    kind='bar',
    x='Pickup_month',
    y='Pickup_cnt',
    hue='Pickup_day',
    palette='colorblind',   # colorblind-friendly palette
    edgecolor='black',
    height=6,
    aspect=1.5              # make plot wider
)

# Update legend position and title
ax.legend.set_title('Day of Week')
ax.legend.set_loc('upper right')

# Add plot title and axis labels
plt.title('Uber Ride Demand Trends by Month and Weekday', fontsize=15, weight='bold')
plt.xlabel('Pickup Month')
plt.ylabel('Number of Pickups')

# Format y-axis with thousands separator
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{y:,.0f}'))

plt.show()
```

### Results

![Uber_Ride_Demand_Trends](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/images/Uber_Ride_Demand_Trends.png)

### Insights

- Ride demand clearly increases over time, with noticeable growth from January through May, before stabilizing slightly in June. This suggests a steady rise in usage during the first half of the year.
- Fridays and Saturdays consistently record the highest number of pickups each month, highlighting weekends as the busiest period for ride demand.
- Mondays show the lowest demand compared to other weekdays, reinforcing the idea that commuting patterns alone do not drive as much activity as social or leisure-related travel later in the week.

## 2. What are the peak demand hours for Uber rides in NYC, and how do they vary across weekdays and weekends?

To explore this, I grouped Uber pickup data from New York City (Jan–Jun 2015) by both the hour of the day and the day of the week. By plotting these trends, it became possible to observe how demand shifts throughout the day and compare weekdays with weekends. This visualization highlights not only the daily peaks but also the differences in behavior between weekdays and weekends, providing a better understanding of urban ride-hailing demand.

View my notebook with detailed steps here: [2_Rush_Hour_by_Weekdays.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/2_Rush_Hour_by_Weekdays.ipynb)

### Visualize Data

```python
# Set Seaborn theme for cleaner style
sns.set_theme(style='ticks')
plt.figure(figsize=(10, 5))

# Create a point plot → each line shows number of pickups per hour for each weekday
g = sns.pointplot(
    data=df_grouped,
    x='Pickup_hour',
    y='size',
    hue='Pickup_day',
    palette='tab10',
    legend=False        # turn off legend, labels will be added manually
)
sns.despine()   # remove top and right borders for a clean look

# Add plot title and axis labels
plt.suptitle('Hourly Ride Demand by Day of Week', fontsize=15, weight='bold')
plt.title('New York City Uber Pickups (Jan–Jun 2015)', fontsize=13, style='italic')
plt.xlabel('Pickup Hour (0–23)')
plt.ylabel('Number of Pickups (rides)')

# Format y-axis with thousands separator
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{y:,.0f}'))

# === Add manual labels at the end of each line (on the right side) ===
texts = []

for i, text in enumerate(df_grouped['Pickup_day'].unique()):
    y_axis = df_grouped[df_grouped['Pickup_hour'] == 23]['size'].iloc[i]
    texts.append(plt.text(23.5, y_axis, text, va='center'))

# Adjust labels to avoid overlapping
adjust_text(texts)

# Shift labels a bit further right for clarity
for t in texts:
    t.set_x(23.8)

plt.show()
```

### Results

![Rush_Hour_by_Weekdays](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/images/Rush_Hour_by_Weekdays.png)

### Insights

- The lowest demand appears consistently around 3–5 AM across all days, when the city is mostly inactive. This shows a universal "quiet period" regardless of the weekday or weekend.
- On weekdays, there is a sharp rise in pickups during morning hours (7–9 AM) that aligns with commuting times, followed by another peak in the late afternoon and evening (5–8 PM). This reflects typical work-related travel behavior.
- Weekends show a different pattern: demand continues to rise steadily throughout the day, reaching the highest levels late at night, especially on Friday and Saturday. This suggests that social and leisure activities drive late-night demand compared to weekday commuting peaks.

## 3. Does the relationship between time of day and month reveal shifting travel behaviors in New York City’s Uber demand?

To uncover this, I mapped Uber pickup data from April to September 2014 into a heatmap that combines two critical dimensions: hours of the day and months of the year. This heatmap highlights not just daily rush hour peaks, but also how those peaks shift over time. By layering the hourly and monthly dimensions, the visualization makes it easier to spot behavioral changes in ride demand and the potential operational challenges or opportunities they create.

View my notebook with detailed steps here: [3_Rush_Hour_by_Months.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/3_Rush_Hour_by_Months.ipynb)

### Visualize Data

```python
ax = sns.heatmap(data=df_pivot, cmap="Blues")

ax.xaxis.set_ticks_position('top')
ax.set_xlabel('')
ax.set_ylabel('Hour of Day')

plt.suptitle('Average Uber Pickups per Hour across Months', fontsize=14, weight="bold", y=1.04)
plt.title('Data aggregated from April to September 2014', fontsize=10, style="italic", pad=30, loc='center')

plt.show()
```

### Results

![Rush_Hour_by_Months](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/images/Rush_Hour_by_Months.png)

### Insights

- Evening demand shows a clear escalation over the months, with September standing out as the strongest. Late afternoon and evening hours (around 5–9 PM) become increasingly saturated compared to earlier months, signaling stronger reliance on Uber for commuting or after-work travel. This suggests a growing need for resource allocation, such as more drivers or surge pricing, in the evening slots as the year progresses.
- Morning demand is relatively stable across months, peaking modestly between 7–9 AM. The consistency here indicates that commuter behavior in the mornings is predictable and less influenced by seasonal shifts, meaning operations can maintain a steady supply strategy for these hours.
- Comparing months, the overall intensity of pickups increases noticeably by September. This trend suggests not only higher adoption of Uber but also possible external drivers like seasonal tourism, events, or changes in public transit usage. Recognizing this ramp-up can help anticipate future spikes and inform marketing strategies or partnerships during high-demand months.

## 4. Can the relationship between driver supply and trip demand reveal how efficiently Uber balanced the two across its New York City bases?

To uncover this relationship, I examined Uber’s active vehicles (supply) and trip volumes (demand) for each base during February 2015. I visualized both metrics by day of the week, applying linear regression to estimate how tightly supply and demand moved together. The slope of each regression line serves as a proxy for demand sensitivity, showing how much trip volume changes for every additional active vehicle.

View my notebook with detailed steps here: [4_Active_Vehicles_vs_Trips_Analysis.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/4_Active_Vehicles_vs_Trips_Analysis.ipynb)

### Visualize Data

```python
# Create FacetGrid scatterplots for each dispatching base
g1 = sns.FacetGrid(
    data=df_grouped,
    col='dispatching_base_number',
    hue='day_of_week',
    col_wrap=3,
    sharex=False, sharey=False,
    height=4
)

# Plot scatter points: active_vehicles vs trips
g1.map(sns.scatterplot, 'active_vehicles', 'trips', alpha=0.7)

# Loop through each facet and add regression line + labels
for ax, base in zip(g1.axes.flat, df_grouped['dispatching_base_number'].unique()):
    df_base = df_grouped[df_grouped['dispatching_base_number'] == base]

    # Compute linear regression slope and intercept
    slope, intercept = np.polyfit(df_base['active_vehicles'], df_base['trips'], 1)

    # Plot regression line
    sns.regplot(
        data=df_base,
        x='active_vehicles',
        y='trips',
        scatter=False,
        ax=ax,
        line_kws={'color':'black', 'alpha':0.6, 'lw':2}
    )
    
    # Add title with slope information
    ax.set_title(f'Base number: {base} (slope={slope:.1f})')

    # Format axis tick labels (in thousands)
    ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{(y/1000):.1f}K'))
    ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{int(y/1000)}K'))

    # Get min and max of x-axis values
    x_min, x_max = df_base['active_vehicles'].min(), df_base['active_vehicles'].max()

    # Ensure consistent number of ticks across facets
    ax.set_xticks(np.linspace(x_min, x_max, 6))

    # Add day-of-week text labels and adjust to avoid overlap
    texts = [ax.text(x+20, y, day, va='center') 
             for x, y, day in zip(df_base['active_vehicles'], df_base['trips'], df_base['day_of_week'])]
    adjust_text(texts, ax=ax)

# Set axis labels for all facets
g1.set_axis_labels('Active Vehicles (Supply)', 'Trips (Demand)')

# Add overall title and subtitle
g1.figure.subplots_adjust(top=0.87)
month_name = df_foil_month['month'].iloc[0]
plt.suptitle(
    f'Uber Active Vehicles vs Trips by Day of Week in New York ({month_name} 2015)',
    fontsize=15,
    weight='bold'
)

# Configure subtitle text and position
g1.figure.text(
    0.5,
    0.93,
    'Regression line represents linear trend between supply and demand, slope shown per base',
    ha='center',
    fontsize=12,
    style='italic'
)

# Add legend for regression line
reg_line = mlines.Line2D([], [], color='black', lw=2, label='Regression Line')
g1.figure.legend(
    handles=[reg_line],
    loc='upper right',
    bbox_to_anchor=(1, 0.95),
    fontsize=12,
    frameon=True
)

plt.show()
```

### Results

![Active_Vehicles_vs_Trips_Analysis](https://github.com/marissawyl/Python_Projects/blob/main/Project_3/images/Active_Vehicles_vs_Trips_Analysis.png)

### Insights

- Across all bases, the regression lines reveal a clear positive relationship between active vehicles and trip volume, meaning that when more drivers are on the road, demand generally rises in tandem. This indicates strong coordination between supply and demand in Uber’s system during the observed period.
- Bases like B02682 and B02617 show steeper slopes (≈13), suggesting higher responsiveness of demand to additional driver supply. These bases likely operated in busier or more central zones, where marginal increases in available vehicles translated quickly into completed trips.
- Weekends, especially Friday and Saturday, consistently appear near the upper end of each trend line, signaling surges in both supply and demand. This pattern implies effective scaling of driver availability to match peak weekend activity, which is an important operational insight for scheduling and incentive design.

# What I Learned

Through this project, I deepened my technical and analytical skills while improving my ability to turn data into meaningful insights. Some of the most valuable lessons I gained include:
- Gaining practical experience in using Pandas for complex data manipulation and Seaborn/Matplotlib for producing clear, data-driven visualizations.
- Learning to work effectively with time-based data, such as extracting and combining month, weekday, and hour information to uncover user behavior trends.
- Understanding the importance of data preprocessing and cleaning, especially when dealing with datetime conversions and ensuring consistency across columns.
- Strengthening my ability to communicate insights visually, focusing not only on the accuracy of data but also on how the story is presented to the audience.
- Developing a more structured workflow, from data wrangling to visualization, to support analytical reasoning and business interpretation.

# Challenges I Faced

This project presented several challenges that tested both technical and creative problem-solving skills:
- Managing datetime transformations was more complex than expected. Extracting and aligning time components required precise logic to avoid aggregation errors.
- Choosing the right visualization structure took trial and error. It was challenging to display multi-dimensional time data without overcrowding the charts.
- Maintaining clarity and readability in Seaborn plots required careful use of color palettes, labels, and formatting to make insights stand out effectively.
- Balancing technical precision with storytelling was another key challenge, including ensuring that the visuals were both analytically accurate and intuitively understandable.

# Conclusion

This project provided a clear understanding of how Uber ride demand varies by month, weekday, and hour across New York City. It revealed strong behavioral patterns, such as higher activity during weekday rush hours and seasonal shifts in overall demand.

Beyond pattern recognition, the analysis showed how data insights can support business and operational strategies, including driver allocation and demand forecasting.

Overall, the experience strengthened my ability to manage an end-to-end analysis pipeline, from data cleaning and feature extraction to visualization and interpretation, while sharpening my understanding of how data analytics can drive real-world decision-making.
