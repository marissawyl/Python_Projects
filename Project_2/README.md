# Introduction

This project continues my journey to strengthen my **Python** and **data analysis** skills. Building on the foundational concepts from my first project, it focuses on a 2024 job postings dataset from _Luke Barousse_ to explore trends and insights from the Indian job market. Along the way, I practiced cleaning, analyzing, and visualizing real-world data, while at the same time learned how to turn those findings into clear, meaningful insights.

If you’d like to see the earlier part of this series, where I laid the groundwork for my Python and data analysis skills, feel free to check out my 👉 [Project 1](https://github.com/marissawyl/Python_Projects/tree/main/Project_1).

# Background

The goal of this analysis is to explore the demand for Data Analyst roles in India in 2024. While Data Engineering positions show the highest demand, I chose to focus on Data Analyst roles for several reasons:  
1. **Learning focus**: This project is primarily about honing data analysis skills, including data cleaning, exploration, and visualization.  
2. **Actionable insights**: Understanding trends in Data Analyst positions can help beginners or career switchers navigate the job market.  
3. **Portfolio relevance**: Focusing on Data Analyst roles allows me to generate insights that are directly applicable to career planning and to create analysis that effectively showcases my skills in a portfolio-ready format.

By focusing on Data Analyst roles, I could concentrate on developing strong analytical and Python skills while still providing meaningful insights about the job market.

# Questions to Analyze

These are the main questions explored in this project:

1. Which data-related job roles are most in demand across the top countries in 2024, and how does their popularity vary by country?
2. What do the 2024 hiring trends reveal about the most sought-after data roles in India?
3. Which data roles in India are most likely to provide remote work options in 2024, and how does this vary across data roles?
4. Which skills are driving demand and top salaries for Data Analysts in India in 2024?
5. What skill combinations are most in demand for data analysts in India in 2024?

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

# Loading Data
df = pd.read_csv(r'D:\Learn Python for Data Analysis\PYTHON_EXERCISES_LUKE\job_postings_flat_from_luke.csv')

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda skills: ast.literal_eval(skills) if pd.notna(skills) else skills)
```

# The Analysis

## 1. Which data-related job roles are most in demand across the top countries in 2024, and how does their popularity vary by country?

To address this question, I first identified the top 3 job roles based on their prevalence in job postings, then compared their distribution across leading countries. By visualizing the likelihood of each role being posted in different regions, I could observe regional trends and high-demand positions, helping inform strategic career or business decisions in the global data industry.

View my notebook with detailed steps here: [1_Roles_by_Country.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/1_Roles_by_Country.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')

g = sns.catplot(
    data=df_merged,
    kind='bar',
    x='job_country',
    y='title_pct',
    hue='job_title_short',
    palette='tab10',
    order=top_countries
)
g._legend.set_title('Job Roles')
g._legend.set_loc('center right')

plt.xticks(rotation=45, ha='right')
plt.xlabel('')
plt.ylabel('Likelihood in Job Posting')
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.title(f'Distribution of Data Job Roles Across Top {top_n} Countries (2024)', fontsize=15)
plt.grid(True, axis='y', linestyle='--', alpha=0.7)

plt.show()
```

### Results

![Roles_by_Country](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/images/Roles_by_Country.png)

### Insights

- Data Engineer roles dominate in India, Canada, and the Netherlands, suggesting a high demand for professionals with engineering and data pipeline skills in these regions. Companies are likely prioritizing building and maintaining data infrastructure over analysis.
- Data Analyst positions are relatively lower in India and Canada, but they are more prominent in the United States and France. This indicates that while analysis skills are important, the need for data engineering expertise is currently driving the market in several countries.
- Data Scientist roles maintain a moderate, steady presence across all countries, showing that while specialized analytics and machine learning skills are valued, the foundational work of managing and engineering data remains a top priority for employers.

## 2. What do the 2024 hiring trends reveal about the most sought-after data roles in India?

To explore this, I examined the monthly distribution of job postings across six major data roles. Looking at the job postings across 2024 makes it possible to see which roles were consistently in demand and which spiked only at certain times of the year. These patterns offer useful guidance for anyone deciding where to focus their skills and career development in the data field.

View my notebook with detailed steps here: [2_Job_Roles_by_Month.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/2_Job_Roles_by_Month.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')

ax = sns.lineplot(data=df_pivot, dashes=False)
sns.despine()

plt.title('Monthly Trends of Data-related Job Postings in India (2024)', fontsize=15)
plt.xlabel('2024')
plt.ylabel('Number of Job Postings')
plt.legend(
    title='Job Roles',
    loc='upper right',
    bbox_to_anchor=(1.4, 1)
)

# Find the max value for each job role
for job_role in df_pivot.columns:
    y_max1 = df_pivot[job_role].max()
    x_max1 = df_pivot[job_role].idxmax()

    ax.scatter(x_max1, y_max1, color='red', zorder=5)
    ax.text(x_max1, y_max1 + 50, y_max1, ha='center', color='red', fontsize=9, fontweight='bold')

plt.show()
```

### Results

![Job_Roles_by_Month](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/images/Job_Roles_by_Month.png)

### Insights

- Data Engineer consistently led the market with the highest number of postings, peaking at 1,771 in August. This trend confirms that engineering expertise in building and maintaining scalable data systems remains the backbone of the data job market in India.
- Senior roles such as Senior Data Scientist and Senior Data Engineer showed steady demand throughout the year, though at a lower volume compared to mid-level positions. Their stability indicates that while fewer openings exist, these roles remain critical for organizations investing in leadership and advanced expertise.
- Data Scientist demand surged notably in August, reaching 1,175 postings, showing that companies heavily prioritized advanced analytics talent mid-year. In contrast, Data Analyst roles peaked earlier in March (641 postings) and then declined, suggesting that entry-level and junior positions are more sensitive to market shifts.

## 3. Which data roles in India are most likely to provide remote work options in 2024, and how does this vary across data roles?

This analysis compares the remote job share for Data Analysts, Data Engineers, and Data Scientists. Understanding this distribution provides valuable context for both job seekers exploring flexible opportunities and organizations evaluating how their offerings align with market expectations.

View my notebook with detailed steps here: [3_Remote_Jobs.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/3_Remote_Jobs.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(1, 3, figsize=(10,3), constrained_layout=True)

for i, role in enumerate(job_roles):
    wfh_count = df_2024_IN[df_2024_IN['job_title_short'] == role]['job_work_from_home'].value_counts().reindex([False, True], fill_value=0)
    ax[i].pie(wfh_count, startangle=90, labels=['Not Remote', 'Remote Available'], autopct='%1.1f%%')
    ax[i].set_title(role)

fig.suptitle('Share of Remote Job Postings by Role in India (2024)', fontsize=15)
plt.show()
```

### Results

![Remote_Jobs](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/images/Remote_Jobs.png)

### Insights

- Data Analysts stand out with the highest proportion of remote-friendly roles at 20.5%. This suggests that companies are more open to offering remote work for analysts compared to other data roles.
- Data Engineers show the lowest availability of remote options, with only 9.7% of postings. This could indicate that engineering work often requires closer collaboration with infrastructure teams, making remote setups less common.
- Data Scientists fall in between, with 11.7% of postings mentioning remote work. While slightly higher than engineers, the gap compared to analysts highlights that analytical roles may be more adaptable to remote environments.

## 4. Which skills are driving demand and top salaries for Data Analysts in India in 2024?

To explore this, I looked at both the most in-demand and highest-paying skills for Data Analysts. I compared the skills that appear most frequently in job postings with those linked to the top salaries. This helps identify which skills can boost employability, which can maximize earning potential, and where to focus for career growth.

View my notebook with detailed steps here: [4_Salary_by_Skills.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/4_Salary_by_Skills.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')
fig, ax = plt.subplots(2, 1)

sns.barplot(
    data=df_grouped_median,
    x='median_salary',
    y='job_skills',
    ax=ax[0],
    hue='median_salary',
    palette='dark:b_r',
    legend=False
)
ax[0].set_xlabel('')
ax[0].set_title('Highest Paid Skills')

for idx, v in enumerate(df_grouped_median['median_salary']):
    ax[0].text(v + 2000, idx, f'${(v/1000):.1f}k', va='center', fontsize=10)

sns.barplot(
    data=df_grouped_cnt,
    x='median_salary',
    y='job_skills',
    ax=ax[1],
    hue='median_salary',
    palette='light:b',
    legend=False
)
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(0, 160000)
ax[1].set_title('Most In-Demand Skills')

for idx, v in enumerate(df_grouped_cnt['median_salary']):
    ax[1].text(v + 2000, idx, f'${(v/1000):.1f}k', va='center', fontsize=10)

for i, axis in enumerate(ax):
    axis.set_ylabel('')
    axis.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

    # Set x-ticks only on the last subplot
    if i != len(ax) - 1:
        axis.set_xticks([])

sns.despine()
plt.suptitle('Top-Paying vs Most Popular Skills for Data Analysts in India (2024)', fontsize=15)

fig.tight_layout()
plt.show()
```

### Results

![Salary_by_Skills](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/images/Salary_by_Skills.png)

### Insights

- High-paying skills like TensorFlow, C, C++, JavaScript, and Jupyter offer significant salary potential for Data Analysts, even though they are not necessarily the most common skills in job postings. This suggests that specializing in these areas can give a competitive edge.
- The most in-demand skills, including SAP, Python, R, SQL, and Excel, are widely sought after in job postings but tend to offer lower median salaries compared to the high-paying skills. This indicates that proficiency in these tools is essential for landing a role, even if they do not maximize earnings immediately.
- There is a clear gap between high salary skills and high demand skills, highlighting a strategic choice for career development: focusing on popular skills may increase employability, while mastering high-paying, less common skills can boost earning potential for specialized roles.

## 5. What skill combinations are most in demand for data analysts in India in 2024?

To explore this question, I mapped out the top skills mentioned in job postings and analyzed how frequently they occur together. By looking at these co-occurrences, the goal was to uncover which skills are not only important on their own but also commonly required in combination. This helps to identify clusters of tools and technologies that data analysts are expected to master, making it clearer where to focus skill development.

View my notebook with detailed steps here: [5_Skill_Coocurrences_Pairwise.ipynb](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/5_Skill_Coocurrences_Pairwise.ipynb)

### Construct the Co-occurrence Matrix

```python
# Select the top n skills and convert to a set for .intersection()
top_skills1 = set([s1 for s1, _ in skills_count.most_common(top_n)])

coocurrence1 = Counter()

for skills_2 in df_2024_IN['job_skills']:
    filtered1 = top_skills1.intersection(skills_2)
    paired1 = combinations(sorted(filtered1), 2)
    coocurrence1.update(paired1)

pairs1, cnt = zip(*coocurrence1.items())
df_pairs1 = pd.DataFrame(pairs1, columns=['Skill_1', 'Skill_2'])
df_pairs1['cnt'] = cnt

df_matrix = df_pairs1.pivot_table(
    index='Skill_1',
    columns='Skill_2',
    values='cnt',
    fill_value=0
)

# Make sure skill names align in both rows and columns before transposing
skill_names = sorted(top_skills1)
df_matrix = df_matrix.reindex(index=skill_names, columns=skill_names, fill_value=0)

# Transpose the matrix to convert it into a full matrix
df_matrix = df_matrix + df_matrix.T
```

### Visualize Data

```python
sns.heatmap(data=df_matrix, cmap='Blues')
plt.title(f"Top {top_n} Skill Co-occurrences for Data Analysts in India (2024)", fontsize=15)
plt.xlabel('')
plt.xticks(rotation=45, ha="right")
plt.ylabel('')
plt.yticks(rotation=0)

plt.show()
```

### Results

![Skill_Coocurrences_Pairwise](https://github.com/marissawyl/Python_Projects/blob/main/Project_2/images/Skill_Coocurrences_Pairwise.png)

### Insights

- The strongest pairing in the dataset is SQL with Python, showing that employers highly value professionals who can combine database management with programming for analysis and automation.
- Excel and Power BI appear frequently together, underlining how traditional spreadsheet expertise is still complemented by modern business intelligence tools for reporting and visualization.
- While cloud platforms like AWS and Azure do appear, their co-occurrence with other skills is relatively lighter compared to SQL, Python, and BI tools, suggesting that cloud expertise is beneficial but not the top priority for most data analyst roles in India.

# What I Learned

Through this second project, I further enhanced both my technical and analytical abilities while deepening my proficiency in Python for data analysis. Key insights and skills developed include:
- Gaining more hands-on experience with advanced Python libraries such as Pandas for complex data manipulation, and Seaborn/Matplotlib for producing precise and insightful visualizations.
- Appreciating the importance of robust data cleaning and preprocessing techniques to ensure accuracy, consistency, and reliability in analytical results.
- Strengthening problem-solving and debugging skills by addressing more intricate challenges, including refining plots for readability and resolving data inconsistencies.
- Applying more sophisticated visual storytelling techniques to convey actionable insights, ensuring that charts and graphs are both informative and intuitive.
- Practicing a comprehensive data analysis workflow, from hypothesis formulation and exploratory data analysis to mapping relationships (e.g., co-occurrence matrices) and documenting findings in a professional portfolio-ready format.
- Exploring new analytical approaches and techniques beyond the first project, building versatility and adaptability in tackling real-world datasets.

# Challenges I Faced

- Exploring diverse visualization techniques: I experimented with multiple types of plots to effectively represent data, including complex charts that were new to me, such as co-occurrence matrices. Initially, translating these relationships into clear visual forms was challenging, but iterative adjustments helped me overcome this.
- Managing complex datasets: Handling larger datasets with inconsistencies, missing values, and non-standard formats required careful preprocessing and testing different approaches to ensure accurate analysis.
- Balancing clarity and complexity in visuals: Creating detailed and informative plots while keeping them readable was sometimes tricky, especially when combining multiple variables or adding annotations. Through trial and error, I learned to optimize chart design for both precision and interpretability.
- Debugging and refining code for complex analysis: Encountering errors or unexpected results during advanced manipulations, like mapping relationships in matrices, taught me patience and problem-solving skills to systematically identify and resolve issues.

# Conclusion

This project offered an in-depth look at the Indian data job market in 2024, uncovering trends in demand, remote work opportunities, and skill combinations that drive both employability and earning potential. Through hands-on analysis, I discovered that building strong foundations in Python, SQL, and core analytical skills is critical, while mastering specialized tools like Power BI, TensorFlow, and cloud platforms can provide a competitive edge. Experimenting with diverse visualizations and complex charts, such as co-occurrence matrices, reinforced the importance of clear, insightful communication. Overall, this experience enhanced my ability to handle end-to-end data workflows, strengthened problem-solving skills, and highlighted the value of continuous learning to adapt to the evolving demands of the data analytics field.
