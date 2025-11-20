# Introduction

This first project marks the starting point of my Python and data analysis journey. Using the 2023 U.S. job postings dataset from _Luke Barousse_, I explored the data job market to understand how different roles, skills, and salaries connect across the field.

The main goal was to find patterns, such as which skills are most in demand, which tools define each role, and how these factors influence earning potential. Throughout this project, I practiced working with real-world data in Python, learning how to clean, analyze, and visualize it effectively to uncover clear insights.

# Background

The 2023 U.S. job market dataset was chosen because it offers one of the largest and most detailed views of data-related roles available online. The United States has a mature and dynamic analytics industry, making it an ideal place to explore real hiring trends and skill demand patterns. Working with this dataset helped me get used to the scale and complexity of real-world data, while practicing the essential steps of cleaning, organizing, and turning it into visual insights that tell a meaningful story about the data profession today.

# Questions to Analyze

These are the main questions explored in this project:

1. What are the most demanded skills for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the most optimal skills to learn for Data Analysts?

# Tools Used

To gain insights into the data analyst job market, I worked with a range of tools and technologies that supported every stage of the project:

- **Python**: Served as the foundation of my analysis, helping me process data and uncover key insights. To extend its capabilities, I incorporated several powerful libraries:
    - **Pandas**: Used for cleaning, transforming, and structuring the dataset efficiently.
    - **Matplotlib**: Helped me build clear, data-driven visualizations.
    - **Seaborn**: Enhanced my charts with refined and professional visuals.
- **Jupyter Notebook**: Provided an interactive space to experiment with code, document findings, and combine visuals with analysis.
- **Visual Studio Code**: Offered a reliable and organized environment for writing and testing scripts.

# Data Preparation and Cleanup

To start the project, I imported the necessary libraries and loaded the dataset into Python. After loading, I performed data cleanup to prepare the dataset for analysis and visualization.

```python
# Importing Libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import ast
from datasets import load_dataset

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda skills: ast.literal_eval(skills) if pd.notna(skills) else skills)
```

# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To figure this out, I first identified the top three roles based on popularity, then extracted the five most in-demand skills for each. The results highlight the leading job titles along with their key skills, helping me understand which areas to focus on depending on the role I want to pursue.

View my notebook with detailed steps here: [1_Skills_Count.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/1_Skills_Count.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_pct[df_skills_pct['job_title_short'] == job_title].head()
    sns.barplot(data=df_plot, x='skill_pct', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r', legend=False)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].set_xlim(0, 78)
    ax[i].set_title(job_title)

    for n, v in enumerate(df_plot['skill_pct']):
        ax[i].text(v + 1, n, f'{v:.1f}%', va='center')
    
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout()
plt.show()
```

### Results

![Skill_Count](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/images/Skills_Count.png)

### Insights

- SQL is the most requested skill for Data Analysts (50.8%) and Data Engineers (68.3%), while Python dominates for Data Scientists (72.0%) and is also highly demanded for Data Engineers (64.9%). This shows that mastering both is essential in any data career path.
- Data Analysts rely more on Excel (40.6%) and Tableau (28.5%) for reporting and visualization, while Data Engineers are expected to know cloud tools like AWS (42.8%) and Azure (32.3%). Data Scientists, on the other hand, complement Python and SQL with R (44.2%) for statistical analysis.
- Data Analysts are typically expected to be proficient in business-oriented tools such as Excel and Tableau, which support reporting and data visualization. Data Engineers, in contrast, are more often required to work with cloud platforms and big data technologies, including AWS, Azure, and Spark, reflecting the technical nature of building and maintaining data infrastructure. Data Scientists, meanwhile, are frequently expected to bring strong statistical expertise through programming languages like R as well as specialized statistical software such as SAS.


## 2. How are in-demand skills trending for Data Analysts?

To explore the skill trends for Data Analysts in 2023, I focused specifically on job postings for that role and organized the required skills based on the month they were listed. This approach highlighted the 5 most in-demand skills each month, providing a clear view of how their popularity shifted over the year.

View my notebook with detailed steps here: [2_Skills_Trend.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/2_Skills_Trend.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')

sns.lineplot(data=df_plot, dashes=False, legend=False, palette='hls')
sns.despine()
plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')

last_values = df_plot.iloc[-1, :]

offsets = {}

# Manually set the labels for 'tableau' and 'python' since they couldn’t be adjusted with adjustText
for i, val in enumerate(last_values):
    floored = math.floor(val)
    if floored not in offsets:
        offsets[floored] = 0
    else:
        offsets[floored] += 2

    y = val - offsets[floored]
    plt.text(11.2, y, df_plot.columns[i], va='center')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### Results

![Skill_Trend](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/images/Skills_Trend.png)

### Insights

- SQL consistently remained the most in-demand skill for Data Analysts throughout 2023, though it showed a slight downward trend over the year. This highlights how SQL continues to be the core technical requirement for analysts despite small fluctuations.
- Excel maintained a stable position as the second most requested skill, showing its continued relevance in day-to-day analysis and reporting tasks. However, it experienced a noticeable dip in the later months before recovering slightly in December.
- Python and Tableau followed a similar pattern with moderate demand, occasionally overlapping each other, while SAS consistently ranked the lowest. This suggests that while programming and visualization skills are valued, traditional tools like SAS are becoming less prioritized compared to modern alternatives.


## 3. How well do jobs and skills pay for Data Analysts?

To determine which roles and skills offer the highest salaries, I focused on job postings from the US and analyzed their median pay. Before diving into the details, I first examined the salary distributions for common data positions such as Data Scientist, Data Engineer, and Data Analyst to understand which roles typically command the highest compensation.

View my notebook with detailed steps here: [3_Salary_Analysis.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/3_Salary_Analysis.ipynb)

### Visualize Data

```python
sns.boxplot(data=df_data_roles, x='salary_year_avg', y='job_title_short', order=median_salary.index)
sns.despine()
plt.xlim(0,600000)
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.title('Salary Distributions of Data Jobs in the US', fontsize=15)

plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

plt.show()
```

### Results

![Salary_Analysis](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/images/Salary_Analysis_1.png)

### Insights

- Senior roles clearly command higher salaries across the board, with Senior Data Scientists and Senior Data Engineers showing the largest median salaries. This highlights how experience and advanced expertise significantly increase earning potential in the data field.
- Among non-senior positions, Data Scientists generally earn more than Data Engineers and Data Analysts, indicating that analytical and modeling skills tend to be valued higher than reporting or infrastructure-focused roles.
- An interesting observation is that Senior Data Analysts still earn less on average than non-senior Data Scientists and Data Engineers. This suggests that technical and modeling expertise in engineering and science roles tend to be rewarded more highly than seniority in purely analytical positions.

## Top-Paying and Most In-Demand Skills for Data Analysts

Next, I refined the analysis to focus exclusively on data analyst positions. I examined both the skills that command the highest salaries and those that are most frequently requested. To illustrate these findings, I presented the results using two bar charts.

### Visualize Data

```python
sns.set_theme(style='ticks')

fig, ax = plt.subplots(2, 1)

sns.barplot(
    data=high_salary,
    x='median',
    y=high_salary.index,
    ax=ax[0],
    hue='median',
    palette='dark:b_r',
    legend=False
)
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_xlabel('')

sns.barplot(
    data=top_skills_salary,
    x='median',
    y=top_skills_salary.index,
    ax=ax[1],
    hue='median',
    palette='light:b',
    legend=False
)
ax[1].set_xlim(0, 200000)
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')

for i in range(2):
    ax[i].set_ylabel('')
    ax[i].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

fig.tight_layout()
plt.show()
```

### Results

![Salary_Analysis](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/images/Salary_Analysis.png)

### Insights

- The skills with the highest salaries for Data Analysts are mostly specialized or niche technologies such as dplyr, Bitbucket, GitLab, Solidity, and Hugging Face. This suggests that analysts who combine core analytical abilities with expertise in modern tools or programming frameworks can command significantly higher pay.
- In contrast, the most in-demand skills are more traditional and widely used, such as Python, Tableau, SQL, R, and Excel. These reflect the essential toolkit for most analyst roles, emphasizing versatility and practical application over specialization.
- The comparison highlights a clear gap between what is most valued in the job market and what is most common in everyday analyst work. While core skills drive employability, niche technical expertise offers a path to higher earning potential.

## 4. What are the most optimal skills to learn for Data Analysts?

To determine which skills provide the greatest value for career growth, I analyzed both their demand in job postings and their median salaries. This made it easier to highlight the skills that offer the best balance of demand and compensation.

View my notebook with detailed steps here: [4_Optimal_Skills.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/4_Optimal_Skills.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')

sns.scatterplot(data=df_plot, x='skill_percent', y='median_salary', hue='technology')
sns.despine()

plt.title('Most Optimal Skills for Data Analysts in the US', fontsize=15)
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary')
plt.legend(title='Technology')

ax=plt.gca()
ax.xaxis.set_major_formatter(PercentFormatter())
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))

# Create a variable list for adjust_text()
texts = []

for i, txt in enumerate(df_plot.index):
    x = df_plot['skill_percent'].iloc[i]
    y = df_plot['median_salary'].iloc[i]
    texts.append(plt.text(x, y + 200, txt))

adjust_text(texts, arrowprops=dict(arrowstyle='->', color='grey', lw=1)) # lw= line width

# Manually adjusted the position of the SAS and Power BI labels to resolve overlap
for t in texts:
    if t.get_text() == "sas":
        t.set_position((t.get_position()[0]+1, t.get_position()[1]+500))
    if t.get_text() == "power bi":
        t.set_position((t.get_position()[0]-1, t.get_position()[1]-1500))

plt.show()
```

### Results

![Optimal_Skills](https://github.com/marissawyl/Python_Projects/blob/main/Project_1/images/Optimal_Skills.png)

### Insights

- Python and SQL stand out as the most optimal skills for Data Analysts, combining high salaries with strong demand across job postings. This shows their importance as core skills that provide both career stability and competitive pay.
- Tools like Tableau and R also offer a good balance between demand and salary, making them valuable additions to a data analyst’s toolkit, especially for visualization and statistical analysis.
- On the other hand, skills such as Word and PowerPoint appear frequently in job postings but are associated with lower salaries, indicating that while they are useful, they do not provide the same level of career leverage as technical or programming skills.

# What I Learned

Through this project, I developed both technical and analytical skills while building a stronger foundation in Python for data analysis. Key lessons from the process include:
- Gaining practical experience with Python libraries such as Pandas for data manipulation and Seaborn/Matplotlib for creating effective visualizations.
- Understanding the critical role of data cleaning and preprocessing to ensure accuracy and reliability in analysis results.
- Strengthening problem-solving skills by debugging issues such as overlapping labels and refining visualizations for clarity.
- Learning to apply visual storytelling principles, making insights more accessible and easier to interpret through clear and structured charts.
- Practicing an end-to-end workflow, from formulating questions and analyzing data to documenting findings and presenting them professionally in a portfolio.

# Challenges I Faced

- **Getting familiar with Python syntax and libraries:** Since I am still new to Python, it took time to understand how Pandas, Matplotlib, and Seaborn work together for data analysis and visualization.
- **Data cleaning and preparation:** Handling missing values, inconsistent formats, and preparing the dataset for analysis was more challenging than expected, but it helped me understand why preprocessing is such an important step.
- **Visualization and designing complex visuals:** Making plots clear and readable, managing overlapping labels, scaling charts properly, and creating complex visualizations that are accurate and easy to interpret was challenging, but essential for delivering insights effectively.

# Conclusion

This project provided a comprehensive view of the data analyst landscape, revealing which skills are most in-demand, how they trend over time, and how they impact earning potential. Through hands-on analysis, I learned that mastering core technical skills like Python and SQL is essential for both employability and competitive salaries, while specialized tools can offer higher financial rewards. Additionally, proficiency in visualization and analytical tools such as Tableau, R, and Excel is crucial for effectively communicating insights. Overall, this experience strengthened my understanding of the end-to-end data workflow, emphasized the importance of data cleaning and thoughtful visualization, and highlighted the need for continuous learning to stay competitive in the evolving field of data analytics.
