# Intro

:bar_chart: Dive into the data job market! 
Focusing on data analyst roles, this project explores 💰 top-paying jobs,:fire: in-demand skills, and 🎢 where high demand meets high salary in data analytics.

:detective:SQL queries? Check them out here: [project_sql folder](/Project_querries/)


# Background
Data comes from [SQL Course](https://lukebarousse.com/sql). 
It is packed with insights like job titles, salaries, locations and skills. 


### The Questions answered: 

1. What are the top-paying DA jobs?
2. What skills are required for those jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I Used
 Several Key Tools were used: 

- **SQL:** The backbone of the analysis
- **PostgreSQL:** The chosen database management system
- **VSCode:** For the database management and the execution of SQL querries
- **Git & GitHub:** Essential for version control and sharing SQL scripts and analysis, ensuring collaboration and project tracking. 


# The Analysis

Each querry focuses on investigatig specific aspects of the data analyst job market.

### 1. Top Paying Data Analyst Jobs in Argentina
To identify these top-paying jobs, I filtered data jobs positions by average yearly salary and location, focusing on jobs whose location was my country, Argentina. I only cared about the month and year of the job posting, so I formatted the job posted date.  This query higlights the top ten high paying opportunities in the field offered in my country of residence. 

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    ROUND(salary_year_avg,0),
    TO_CHAR(job_posted_date, 'YYYY-MM') AS year_month,
    company_dim.name AS Company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON company_dim.company_id = job_postings_fact.company_id
WHERE
    job_location Like '%Argentina%' 
    --AND job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10
```

With the help of ChatGPT, key insights were derived from the data. These are the following: 

- Top Paying Roles 💸

The highest paying data-related roles are "Senior Data Engineer" with an annual salary of \$147,500.
Data Engineer and Data Scientist Manager are other top-paying positions, with salaries of \$147,500 and $132,500 respectively.
- Job Locations 🗺️

Buenos Aires is a major hub for high-paying data jobs, hosting roles such as Senior Data Engineer, Data Engineer, and Data Scientist Manager.
There are also high-paying roles in other locations like Vicente López and Rosario, indicating demand across different parts of the country.
- Companies Offering Top Salaries 🤑

Prominent companies offering high salaries include MediaLab, Software Mind, Dialpad, Syngenta Group, and Visa.
International companies like Visa and Nestlé highlight the global interest in data professionals in Argentina.
- Job Schedule Types ⏲️

The majority of top-paying jobs are full-time positions, emphasizing that full-time roles tend to offer higher salaries in the data field.
There is a notable contractor role, "Senior Data Engineer" at MediaLab, with a competitive salary of $147,500.
- Salary Range:

The salaries for data-related roles range from $65,000 to $147,500 annually.
High variance in salaries shows different levels of demand and skill requirements across various job titles.
- Posting Trends 📈

Job postings span across multiple months in 2023, indicating a steady demand for data professionals throughout the year.
The most recent posting in the dataset is from December 2023, for roles like "Digital Data Engineer" and "Data Analyst."
- Variety of Roles 🧑‍💼

The dataset includes diverse data-related roles such as Data Engineers, Data Scientists, Data Analysts, Machine Learning Engineers, and Business Intelligence Experts.
This variety shows the broad range of skills and expertise required in the data job market.
- Specialized Roles :construction_worker:

Specialized roles like "LAS Seeds Data Engineer" and "Digital Data Engineer" suggest niche areas within the data field that offer competitive salaries.
Roles like "Data Scientist Manager" and "Business Intelligence | Experts/ Managers" indicate opportunities for leadership positions in the data domain.


These insights provide a comprehensive overview of the data job market in Argentina, highlighting key roles, locations, companies, and salary trends.

![Top Paying Roles](assets\image_top_paying_roles.png)

*Bar graph visualizing the results obtained from the analysis done with ChatGPT.*

### 2.Top paying skills
To identify what where the skills that paid the most amount of money, I began searching for those skills that were necesssary for the top ten highest-paying jobs from the first querry. 

```sql
WITH top_paying_jobs AS(
    SELECT
        job_id,
        job_title,
        job_location,
        job_schedule_type,
        salary_year_avg,
        job_posted_date,
        company_dim.name AS Company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON company_dim.company_id = job_postings_fact.company_id
    WHERE
        job_location Like '%Argentina%' 
        --AND job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10
)

SELECT top_paying_jobs.*,
        skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_dim.skill_id = skills_job_dim.skill_id
ORDER BY
    salary_year_avg DESC


```

These are the insights that ChatGPT unearthed from this querry: 

Insights on Skills for Top-Paying Data Jobs
 - Common Skills Across Roles:

    - Python: Essential for multiple roles, indicating its importance in data engineering and data science.
    - Spark: Frequently required, highlighting its role in big data processing.
    - AWS and GCP: Popular cloud platforms, essential for handling data storage and processing in a scalable manner.
    - SQL and PostgreSQL: Core skills for data management and manipulation.
- Specific Role Requirements:

    - Senior Data Engineer: Requires a broad skill set including SQL, Python, PostgreSQL, Spark, Airflow, Kafka, Linux, Kubernetes, Docker.
    - Data Scientist Manager: Skills in Python, R, and Spark are critical, showing a mix of programming and big data tools.
    - Data Engineer at Dialpad: Emphasizes skills in Python, Scala, Java, Azure, Databricks, AWS, BigQuery, Redshift, GCP, Spark, and GDPR compliance.
- DevOps and DataOps Skills:

    - Tools like Kubernetes, Docker, Jenkins, and GitLab are important, indicating the need for CI/CD and containerization knowledge in data roles.
- Big Data Tools:

    - Technologies like Hadoop, Kafka, and Databricks are essential, reflecting the necessity to handle large-scale data processing.

### 3. Top demanded skills
Now I focused on the demand for the different skills for Data Analyst roles, since these are the most common entry-level roles in the data world. 

```sql
SELECT 
    skills,
    COUNT(job_postings_fact.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_dim.skill_id = skills_job_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND
    job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5
```

Based on the analysis of the demand for skills required to become a Data Analyst in the global job market, the following key insights were derived:

| Skill       | Demand Count | 
|-------------|---------------|
| SQL         | 7,291         | 
| Excel       | 4,611         | 
| Python      | 4,330         | 
| Tableau     | 3,745         | 
| Power BI    | 2,609         | 
| R           | 2,011         | 
| SQL Server  | 1,799         | 
| SAS         | 1,561         | 
| Hadoop      | 1,350         | 
| Google Analytics | 1,231    | 
*Table with the demand count of the different Data Analyst required skills*


- SQL: The most in-demand skill for Data Analysts, with 7,291 job postings requiring proficiency in SQL. This highlights the importance of SQL for data manipulation, querying databases, and managing large datasets.

- Excel: The second most sought-after skill, with 4,611 job postings. Excel remains a fundamental tool for data analysis, reporting, and visualization, showing its continued relevance in the job market.

- Python: Python is required in 4,330 job postings, indicating its significance for data analysis, scripting, and automation. Python's versatility and extensive libraries make it a valuable skill for Data Analysts.

- Tableau: Tableau is mentioned in 3,745 job postings, demonstrating its importance for data visualization. Tableau enables analysts to create interactive and insightful visualizations, which are crucial for data-driven decision-making.

- Power BI: Power BI appears in 2,609 job postings, reflecting its role in business intelligence and data visualization. Power BI is used for creating dashboards and reports, helping organizations make data-informed decisions.

These insights provide a comprehensive overview of the essential skills required for Data Analysts in the global job market, emphasizing the importance of SQL, Excel, Python, and data visualization tools like Tableau and Power BI.

### 4. Top Paying Skills
The analysis is very similar to the one in point 3. Now the focus is on the AVG aggregation function, to know what is the average yearly salary for the different skills, required for the remote Data Analyst positions. 

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg),0) AS average_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_dim.skill_id = skills_job_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND
    salary_year_avg IS NOT NULL
    AND 
    job_work_from_home = TRUE
GROUP BY
        skills
ORDER BY
    average_salary DESC
LIMIT 25

```
Insights:

- Highest Paying Skills:

- PySpark: The highest paying skill with an average salary of $208,172. PySpark is crucial for big data processing and analysis using Apache Spark.
- Bitbucket: Averages $189,155, highlighting its importance for version control and source code management in collaborative environments.
- Couchbase and Watson: Both have average salaries of $160,515, indicating high demand for expertise in NoSQL databases and IBM's AI and data platform respectively.

- Important Data Analysis and Engineering Tools:

    - Pandas: Essential for data manipulation and analysis, with an average salary of $151,821.
    - Jupyter: Averages $152,777, underlining its importance for creating and sharing interactive notebooks.
    - NumPy: An important skill for numerical computing, with an average salary of $143,513.
    - Databricks: With an average salary of $141,907, it highlights the demand for skills in unified data analytics platforms.
- Other High-Paying Skills:

    - GitLab: An important tool for CI/CD and version control, with an average salary of $154,500.
    - Swift: A programming language used primarily for iOS development, averaging $153,750.
    - Elasticsearch and Golang: Both average $145,000, reflecting their importance in search engine technologies and programming languages.
- Cloud and DevOps Skills:

    - GCP (Google Cloud Platform): Averages $122,500, showing the demand for cloud computing expertise.
    - Kubernetes: An important skill for container orchestration, with an average salary of $132,500.
    - Jenkins: Essential for CI/CD pipelines, averaging $125,436.
    - Airflow: Used for workflow automation and scheduling, with an average salary of $126,103.

These insights highlight the diverse range of high-paying skills required in the data analysis field, emphasizing the importance of big data tools, data manipulation libraries, cloud platforms, and DevOps technologies. ​

### 5. Optimal Skills

I proceeded to combine the analysis done in 3. and 4. to finally convey the demand and the mean salary for the different skills, helping me decide what combination of them is best to learn, in order to secure a Data Analyst role. 
Bear in mind that this time I only care about the skills that are demanded in 10 or more jobs postings, so that I get to evaluate the skills that are demanded in various jobs, focusing in their relevance as well.  

```sql
SELECT
    skills_dim.skill_id,
    skills,
    COUNT(job_postings_fact.job_id) AS demand_count,
    ROUND(AVG(salary_year_avg),0) AS average_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON skills_job_dim.job_id = job_postings_fact.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND
    job_work_from_home = TRUE
    AND
    salary_year_avg IS NOT NULL
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(job_postings_fact.job_id) > 10
ORDER BY
    average_salary DESC,
    demand_count DESC
    
LIMIT 25

```
The insights derived from this final querry are the following, aided by ChatGPT data analysis capabilites.

| Skill       | Demand Count | Average Salary |
|-------------|---------------|----------------|
| Go          | 27            | $115,320       |
| Confluence  | 11            | $114,210       |
| Hadoop      | 22            | $113,193       |
| Snowflake   | 37            | $112,948       |
| Azure       | 34            | $111,225       |
| BigQuery    | 13            | $109,654       |
| AWS         | 32            | $108,317       |
| Java        | 17            | $106,906       |
| SSIS        | 12            | $106,683       |
| Jira        | 20            | $104,918       |
*Table with the top ten highest paying skills*
- Go: The highest-paying skill with an average salary of $115,320. Go is highly valued for its efficiency and performance in software development.
Confluence: With an average salary of $114,210, indicating the importance of collaboration and documentation tools in project management.
- Hadoop: Averaging $113,193, it reflects the demand for big data processing and storage expertise.
- Snowflake: Averages $112,948, highlighting the significance of cloud data warehousing skills.
- Azure: With an average salary of $111,225, showing the demand for cloud platform proficiency.
- BigQuery: Averaging $109,654, reflecting the importance of Google's big data processing tools.
- AWS: With an average salary of $108,317, indicating the high demand for Amazon's cloud platform skills.
- Java: Averages $106,906, showing its continued relevance in software development.
- SSIS: With an average salary of $106,683, highlighting the need for SQL Server Integration Services expertise.
- Jira: Averaging $104,918, indicating the importance of issue and project tracking tools.
These insights provide a comprehensive overview of the top-paying skills in the job market, emphasizing the importance of cloud platforms, big data tools, and software development skills. ​

# What I Learned
Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

- **🎲 Complex querry crafting:** Mastered the art  of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.
- **📊Data Aggregation:** Got cozy with GROUP BY and turned aggregate functions like COUNT() and AVG() into my data-summarizing sidekicks.  
- **💡 Analytical Wizardry:** Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.


# Conclusions
**Salaries 🧑‍💼** 
 - The highest paying roles include "Senior Data Engineer" with an annual salary of $147,500, followed by "Data Engineer" and "Data Scientist Manager." Buenos Aires is a major hub for these high-paying jobs, but opportunities also exist in other locations like Vicente López and Rosario. Prominent companies offering these top salaries include MediaLab, Software Mind, Dialpad, Syngenta Group, and Visa. Most of these high-paying roles are full-time positions, although there are notable contractor opportunities as well. Job postings span across 2023, indicating a steady demand for data professionals.

**Skills 📐**
- In terms of skills, common requirements across these top-paying roles include proficiency in Python, Spark, AWS, GCP, SQL, and PostgreSQL. Specific roles demand a broader skill set; for instance, "Senior Data Engineer" roles require expertise in SQL, Python, PostgreSQL, Spark, Airflow, Kafka, Linux, Kubernetes, and Docker. DevOps and DataOps skills, such as knowledge of Kubernetes, Docker, Jenkins, and GitLab, are also important, reflecting the need for CI/CD and containerization knowledge. Big data tools like Hadoop, Kafka, and Databricks are essential for handling large-scale data processing. These insights highlight the diverse skills and expertise required to excel in the highest-paying data jobs in Argentina.

# Final Thoughts

Finally, I want to summarize my experience pushing through this challenging yet rewarding SQL course. 

To expand on my background, I started looking for jobs a couple of months ago, and I stumbled into the difficulties that the argentinian job market presents. Since I studied Industrial Engineering, the range of jobs that I applied for ranged from on-site factory jobs to highly administrative roles, neither of which I felt particularly interested in. 

Luckily, I found the possibility of diving into the world of data, and I find this course the stepping stone to begin my journey of learning how to analyze and extract valuable insights from data. This knowledge complemented with my strong analytical background will help propel me into a data related role, a field that excites me. 

Is there a better way to do this, than by analysing real-world data of the job postings of the Data Analytics field?
