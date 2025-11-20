# Introduction

This project wraps up my four-part data analysis learning series. Like the earlier ones, it continues my hands-on practice with Python: exploring, cleaning, and visualizing real-world datasets to tell stories through data. Continuing from my previous projects — especially Project 3, which explored Uber trip data from New York City — this time I turned my attention to Bitcoin’s market movement. The dataset used here contains Bitcoin’s historical prices from 2013 to July 2017, originally shared by _Shan Singh_ on Udemy. Through this project, I wanted to look closer at how Bitcoin’s price evolved during its early years before entering the global spotlight in late 2017.

If you’d like to see the previous project in this series, feel free to check out my [Project 3](https://github.com/marissawyl/Python_Projects/tree/main/Project_3).

# Background

Bitcoin’s early years were full of fast growth, big price swings, and growing global interest. From 2013 to mid-2017, its price showed how a small digital idea started turning into something the world began to notice.

In this project, I explored how Bitcoin’s price changed during that time — looking at both the long-term trend and short-term ups and downs. The visualizations highlight key parts of its market behavior: overall growth, daily returns, the link between price and trading volume, and one detailed look at 2016. The results show how Bitcoin’s market evolved during its early phase, offering a data-driven view of its growth and fluctuations.

# Questions to Analyze

These are the main questions explored in this project:

1. What patterns emerge from price movements between 2013 and 2017, with a focus on July 2017?
2. What can Bitcoin’s daily returns from 2013 to 2017 tell us about how its volatility evolved during its early growth years?
3. Does higher Bitcoin trading volume tend to move hand in hand with stronger daily returns?
4. How did Bitcoin’s price move during 2016 (a year that set the stage before its big boom in 2017)?

# Tools Used

To gain insights into the data analyst job market, I worked with a range of tools and technologies that supported every stage of the project:

- **Python**: Served as the foundation of my analysis, helping me process data and uncover key insights. To extend its capabilities, I incorporated several powerful libraries:
    - Pandas: Used for cleaning, transforming, and structuring the dataset efficiently.
    - Plotly: Applied to build interactive charts and explore market trends.
        - `plotly.express`: helped create quick, high-level visuals.
        - `plotly.graph_objects`: allowed for more detailed and customizable plots.
- Jupyter Notebook: Provided an interactive space to experiment with code, document findings, and combine visuals with analysis.
- Visual Studio Code: Offered a reliable and organized environment for writing and testing scripts.

I chose Plotly because I wanted to experiment with interactive charts. It allowed me to explore data dynamically, such as zooming, hovering, and comparing points, which made spotting trends and anomalies a lot easier. It was also a great chance to practice building visuals that feel more engaging and insightful.

# Data Preparation and Cleanup

To start the project, I imported the necessary libraries and loaded the dataset into Python. After loading, I performed data cleanup to prepare the dataset for analysis and visualization.

```python
# Importing Libraries
import pandas as pd
import plotly.graph_objects as go
import plotly.express as px
from plotly.subplots import make_subplots

# Loading Data
df = pd.read_csv(r'D:\Data Analyst stuffs\Python stuffs\Learn Python for Data Analysis\PYTHON_EXERCISES_UDEMY\Bitcoin Case Study\bitcoin_price_Training - Training.csv')

# Convert the datetime from str to datetime
df['Date'] = pd.to_datetime(df['Date'])

# Set 'Date' as index
df = df.set_index('Date').sort_index()
```

# The Analysis

## 1. What patterns emerge from price movements between 2013 and 2017, with a focus on July 2017?

To explore this, I visualized historical price data from 2013 to 2017. The upper chart shows quarterly average closing prices, capturing the overall growth pattern, while the candlestick chart below zooms into July 2017 to highlight daily fluctuations. I chose to focus on this month because it follows a sharp rise that began earlier in 2017, making it an interesting point to observe how the market behaved after that strong upward shift.

As background context, 2017 was a significant year for Bitcoin’s visibility and global market sentiment. The year saw a broad economic recovery, growing public attention on cryptocurrencies, and increased speculative interest as prices surged throughout the year. While these factors may have influenced the market, this analysis remains descriptive and focuses solely on the observable price movements shown in the data.

View my notebook with detailed steps here: [1_Closing_Price_Movements_Analysis.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/1_Closing_Price_Movements_Analysis.ipynb)

### Visualize Data

```python
# Create a line chart showing quarterly average closing prices
fig_line = px.line(
    data_frame=quarterly_resample,
    x=quarterly_resample.index,
    y='Close'
)

# Create a candlestick chart for daily price movements in July 2017
fig_candlestick = go.Candlestick(
        x=df_july.index,
        open=df_july['Open'],
        high=df_july['High'],
        low=df_july['Low'],
        close=df_july['Close']
)

# Create a subplot layout with 2 rows and 1 column:
# - Row 1: quarterly trend line
# - Row 2: detailed July candlestick chart
fig = make_subplots(
    rows=2, cols=1,
    shared_xaxes=False,
    subplot_titles=('Quarterly Average Closing Price (2013–2017)', 'Daily Price Movements – July 2017')
)

# Add the line chart traces into the first subplot
for trace in fig_line.data:
    fig.add_trace(trace, row=1, col=1)

# Highlight the period Apr–Jul 2017 with a red shaded rectangle
fig.add_shape(
    type="rect",
    x0="2017-03-31", x1="2017-07-31",
    y0=0,
    y1=quarterly_resample['Close'].max(),
    xref="x1", yref="y1",    # use x and y axis of line chart (row=1, col=1)
    fillcolor="red",
    opacity=0.2,
    layer="below",
    line_width=0
)

# Add the candlestick chart to the second subplot
fig.add_trace(fig_candlestick, row=2, col=1)

# Adjust height, remove legend, disable candlestick range slider
fig.update_layout(
    height=800,
    showlegend=False,
    title='Price Trends Overview: 2013–2017 with Detailed July 2017 Candlestick',
    xaxis2_rangeslider_visible=False, # Remove the slider of candlestick plot
    template='plotly_white'
)

fig.show()
```

### Results

*Note: Plot displayed as a static image as Plotly interactive charts cannot be rendered directly on GitHub*

![Closing_Price_Movements_Analysis](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/images/Closing_Price_Movements_Analysis.png)

### Insights

- From 2013 to 2016, prices moved gradually with moderate shifts, but starting in early 2017, the growth line steepened sharply, hinting at rising confidence or new market catalysts.
- Within July 2017, daily price movements were highly volatile, with sharp dips followed by strong recoveries — indicating active trading sentiment and possible speculation during the rapid growth phase.
- The recovery toward the end of July aligned with the broader upward trajectory, suggesting continued positive sentiment throughout that period.

## 2. What can Bitcoin’s daily returns from 2013 to 2017 tell us about how its volatility evolved during its early growth years?

To dig into this, I analyzed daily return data from 2013 to 2017 to track how price fluctuations changed through time. The histogram visualizes the overall distribution of daily returns, showing how often small versus large price movements occurred. Meanwhile, the yearly boxplots compare volatility patterns across different periods, helping pinpoint when the market experienced sharper or steadier movements. The combined analysis offers a more comprehensive view of Bitcoin’s volatility dynamics throughout its early expansion period.

View my notebook with detailed steps here: [2_Volatility_Analysis.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/2_Volatility_Analysis.ipynb)

### Visualize Data

```python
# Create histogram of daily returns to visualize overall distribution
fig_histo = go.Histogram(
    x=df['Close_price_pct'],
    nbinsx=100, # Number of bin
    name='Daily Return Distribution',
    marker=dict(
        line=dict(
            color='black',
            width=1
        )
    )
)

# Create boxplot of daily returns per year to compare volatility across time
fig_boxplot = go.Box(
    x=df.index.strftime('%Y'),
    y=df['Close_price_pct'],
    boxmean='sd'    # add standard deviation line
)

# Create a subplot layout
fig = make_subplots(
    rows=2, cols=1,
    shared_xaxes=False,
    subplot_titles=('Distribution of Daily Returns from 2013 to 2017', 'Yearly Volatility Comparison of Daily Returns')
)

# Create a subplot layout
fig.add_trace(fig_histo, row=1, col=1)
fig.add_trace(fig_boxplot, row=2, col=1)

fig.update_layout(
    height=800,
    showlegend=False,
    title='Bitcoin Price Dynamics: Long-Term Trends and Short-Term Volatility (2013–2017)',
    template='plotly_white'
)

# Add axis labels to histogram
fig.update_xaxes(title_text='Daily Return (%)', row=1, col=1)
fig.update_yaxes(title_text='Frequency', row=1, col=1)

# Add axis labels to boxplot
fig.update_xaxes(title_text='Year', row=2, col=1)
fig.update_yaxes(
    title_text='Daily Return (%)',
    range=[-20, 20],    # set the y-axis values
    row=2, col=1
)

fig.show()
```

### Results

*Note: Plot displayed as a static image as Plotly interactive charts cannot be rendered directly on GitHub*

![Volatility_Analysis](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/images/Volatility_Analysis.png)

### Insights

- The histogram shows a clear concentration of daily returns around zero, but with noticeable fat tails on both sides — meaning that while most price changes were modest, extreme movements were not uncommon.
- Yearly comparisons reveal that volatility remained present throughout the 2013–2017 period, though 2013 and 2017 stand out with wider ranges and more outliers, suggesting periods of heightened speculative activity.
- Despite large swings, the median daily returns across years stayed close to zero, indicating that Bitcoin’s dramatic growth phases were driven more by clusters of large positive days than by a steady upward drift.

## 3. Does higher Bitcoin trading volume tend to move hand in hand with stronger daily returns?

To explore this, I plotted daily close-to-close returns against the logarithmic scale of Bitcoin’s daily trading volume. By combining both Pearson and Spearman correlation values, I aimed to capture not just linear but also rank-based relationships between the two metrics. This visualization helps illustrate whether larger trading activity corresponds to stronger price movements or if volume and return patterns follow separate paths.

View my notebook with detailed steps here: [3_Return_Volume_Correlation_Analysis.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/3_Return_Volume_Correlation_Analysis.ipynb)

### Calculate the Pearson and Spearman values

```python
# Calculate correlation between trading volume and daily returns
pearson_val = (
    df_2014_2017[['Volume', 'Close_return']]
    .corr(method='pearson')
    .loc['Volume', 'Close_return']
)

spearman_val = (
    df_2014_2017[['Volume', 'Close_return']]
    .corr(method='spearman')
    .loc['Volume', 'Close_return']
)
```

### Visualize Data

```python
# Create scatter plot: Volume (log scale) vs Daily Returns
fig_scatter = go.Scatter(   
    x=np.log10(df_2014_2017['Volume']), # log scale reduces skew in Volume data
    y=df_2014_2017['Close_return'],
    mode='markers',
    marker=dict(size=6, color='blue', opacity=0.6)
)

# Build figure
fig = go.Figure(fig_scatter)

# Customize layout
fig.update_layout(
    height=600,
    width=1000,
    title=f'Relationship Between Daily Trading Volume and Bitcoin Daily Returns<br><sub>Pearson={pearson_val:.3f}, Spearman={spearman_val:.3f}</sub>',
    xaxis_title='Logarithmic Scale of Daily Trading Volume (BTC)',
    yaxis_title='Daily Close-to-Close Returns (%)',
    template='plotly_white'
)

fig.show()
```

### Results

*Note: Plot displayed as a static image as Plotly interactive charts cannot be rendered directly on GitHub*

![Return_Volume_Correlation_Analysis](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/images/Return_Volume_Correlation_Analysis.png)

### Insights

- The scatter plot shows a wide spread of points without a clear upward or downward trend, reflecting that daily trading volume and returns don’t have a strong direct relationship.
- Both correlation coefficients (Pearson = 0.060, Spearman = 0.138) confirm this weak association, which suggesting that Bitcoin’s price swings weren’t primarily driven by short-term changes in trading volume.
- The slight positive skew hints that on some days, higher volumes coincide with larger returns, but this pattern isn’t consistent enough to indicate a stable correlation.

## 4. How did Bitcoin’s price move during 2016 (a year that set the stage before its big boom in 2017)?

To explore this, I focused on Bitcoin’s 2016 price data and identified its largest single rally and drawdown within the year. The line chart shows the daily closing prices, with key points marked to highlight both extremes: a sharp 33.4% surge and a 28.6% drop. This simple view helps capture how quickly the market could swing during Bitcoin’s early adoption phase, capturing the tension between optimism and correction in a single year.

View my notebook with detailed steps here: [4_Crash_Rally_Analysis.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/4_Crash_Rally_Analysis.ipynb)

### Visualize Data

```python
# ---- Plotting ----
fig = go.Figure()

# Line plot of closing prices
fig_line = go.Scatter(
    x=df_2016.index,
    y=df_2016['Close'],
    mode='lines',
    name='Close Price'
)

# Mark the largest drawdown
fig_drawdown = go.Scatter(
    x=[drawdown_max_idx],
    y=[df_2016.loc[drawdown_max_idx, 'Close']],
    mode='markers+text',
    marker=dict(color='red', size=10),
    text=f'Crash {drawdown_max_val*100:.1f}%',
    textposition='bottom center',
    name='Largest Drawdown'
)

# Mark the largest rally
fig_rally = go.Scatter(
    x=[rally_max_idx],
    y=[df_2016.loc[rally_max_idx, 'Close']],
    mode='markers+text',
    marker=dict(color='green', size=10),
    text=f'Rally {rally_max_val*100:.1f}%',
    textposition='top center',
    name='Largest Rally'
)

# Add all plots to the figure
fig.add_traces([fig_line, fig_drawdown, fig_rally])

fig.update_layout(
    title='Bitcoin Price Movements with Largest Crash and Rally'\
          '<br><sub>Case Study: Year 2016</sub>',
    xaxis_title='Date',
    yaxis_title='Closing Price (USD)'
)

fig.show()
```

### Results

*Note: Plot displayed as a static image as Plotly interactive charts cannot be rendered directly on GitHub*

![Crash_Rally_Analysis](https://github.com/marissawyl/Python_Projects/blob/main/Project_4/images/Crash_Rally_Analysis.png)

### Insights

- The largest rally occurred around mid-2016, with prices surging more than 30% within a short period, signaling renewed investor confidence and momentum in Bitcoin’s growth.
- Not long after, the market experienced a significant 28.6% drop, underlining how volatile the asset remained even as it gained mainstream attention.
- Despite both extremes, Bitcoin closed the year near its highs, showing that 2016 marked a turning point toward broader recovery and sustained upward momentum.

# What I Learned

Through this final project, I deepened my understanding of both technical and analytical aspects of data visualization using Python. It provided a valuable opportunity to integrate the knowledge I had gained from earlier projects and apply it to a real financial dataset. Key lessons and skills I developed include:
- Strengthening my ability to work with real-world time series data: cleaning, transforming, and organizing it for visual analysis.
- Learning how to use both `plotly.express` for quick visuals and `plotly.graph_objects` for more control and customization.
- Gaining a deeper understanding of volatility and momentum through data, which is not from finance theory, but by actually visualizing it.
- Building a better sense of data storytelling flow: connecting multiple charts (trend, volatility, and volume) into a single, coherent analysis that’s easy to follow.
- Practicing visual storytelling: focusing on how to guide viewers through complex trends clearly, without overwhelming them with too much data.

# Challenges I Faced

- This was my first time using Plotly, so I needed time to get used to how it works. Some things that were simple in Matplotlib felt different here, especially when setting up the layout and styling.
- Getting the charts to show Bitcoin’s sharp price changes clearly was harder than I expected. I had to adjust scales and formats a few times to make the movement look right.
- Putting the different views together into one smooth story also took some planning, especially to keep everything easy to follow.

# Conclusion

This project wrapped up my Python visualization journey by combining everything I had practiced, from data cleaning and transformation to building interactive charts. Working with Bitcoin’s historical price data gave me a deeper look into how data storytelling can uncover patterns behind market behavior, even without advanced financial models.

Beyond improving my technical skills, this project helped me appreciate the creative side of visualization, which is finding balance between clarity and depth while turning raw data into an understandable story. It also showed how visualization can make something as complex as market volatility easier to interpret, even for a general audience.

Completing this project felt like a full-circle moment. It brought together coding, exploration, and visual communication in a way that reflects how much my analytical thinking has grown since the first project.
