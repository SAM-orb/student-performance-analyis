Overview
This phase of the analysis explores the core drivers of student performance, blending demographic, behavioral, and academic variables. 
The exploration was conducted using a hybrid approach: SQL for complex segmentation and outlier detection, and Excel (Pivot Tables) for data summaries and charts.

1. Data Validation & Outlier Investigation
Before beginning the exploratory analysis, additional validation checks were performed on the cleaned dataset.
# Key Finding
A group of 38 students recorded a final grade of 0.00.
Further investigation strongly suggested that these scores were caused by a systemic data collection or coding issue rather than actual academic failure.
They all had 0 abscences and all other attributes did not follow the pattern of low grade students.
# Actions Taken
Isolated the affected records
Investigated their behavioral and academic attributes
Determined the values were inconsistent with the overall dataset behavior
Removed the records to preserve analytical integrity
# SQL query used
SELECT *
FROM student_data
WHERE final_grade = 0;
![Outlier data](images/.png)

2. Feature Enngineering
To improve the analysis, additional derived columns were created.
A new column called parent_education was created to represent the highest educational level attained by either parent.
I achieved this on google sheets using the formula:

=ARRAYFORMULA(IFS(
  MAX(SWITCH(H2:H, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0), SWITCH(I2:I, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0)) = 4, "Higher",
  MAX(SWITCH(H2:H, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0), SWITCH(I2:I, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0)) = 3, "Secondary",
  MAX(SWITCH(H2:H, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0), SWITCH(I2:I, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0)) = 2, "5th_to_9th grade",
  MAX(SWITCH(H2:H, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0), SWITCH(I2:I, "Higher", 4, "Secondary", 3, "5th_to_9th grade", 2, "Primary", 1, 0)) = 1, "Primary",
  TRUE, "None"
))

3. Student Demographics
Students residing in urban areas account for approximately 78% of the dataset.
Due to this significant demographic imbalance, the address attribute was excluded from the performance analysis to avoid skewed results.
![address chart](images/.png)

4. Academic performance analysis
# Absences vs student performance
Students with higher absenteeism generally had lower average grades.


# Age vs student performance
Student performance appears to be inversely proportional to age.
Older students generally recorded lower average grades.
Older students also showed higher absenteeism rates.
## SQL query used
SELECT age,
       ROUND(AVG(final_grade),2) AS average_grade,
       ROUND(AVG(absences),2) AS average_absences
FROM student_data
GROUP BY age
ORDER BY age;
## Interpretation
Higher absenteeism among older students may contribute to reduced academic performance.

# Studytime vs student performance
Increased study time positively correlates with student performance.
Study time appears more influential than paid tutoring.
## Interpretation
Consistent independent study habits contributes more to academic success than paid tutoring.

# Previous failures vs student performance
Students with previous academic failures consistently performed poorly.
## Interpretation
Past academic struggles may predict future academic difficulties.

5. Family and social factors
# Parent education vs student performance
Student performance positively correlates with:
-Mother's education
-Father's education
-Overall parent education
## SQL query used
SELECT parent_education,
       ROUND(AVG(average_grade),2) AS average_grade
FROM student_data
GROUP BY parent_education
ORDER BY parent_education;
## Interpretation
Higher parental educational attainment may provide stronger academic support environments.

# Romantic status vs student performance
Romantic relationship status showed no significant impact on academic performance. They had a slightly lower mean averag grade than single students
Students in relationships recorded higher absenteeism.

6. Lifestyle and behavioural factors
# Going out frequency vs student performance
Higher social activity (go_out) negatively correlates with student performance.
Also students with high social activity were found to have lower study time.
## Interpretation
Excessive social outings may reduce study time and academic focus.

#Alcohol consumption vs student performance
Students with higher alcohol consumption generally recorded lower academic performance.
## Interpretation
Lifestyle choices may significantly impact educational outcomes.

7. Educational support factors
# Paid tutoring vs student performance
Paid tutoring did not significantly improve overall student performance.
## Interpretation
Academic success may depend more on study habits and motivation than external tutoring.

8. Higher education intention
Students intending to pursue higher education performed better academically.
These students also have higher average study time.
## Interpretation
Long-term educational goals positively influence academic discipline and performance.


  `  KEY CORRELATIONS IDENTIFIED`
Attribute	Relationship with Performance
Study Time	Positive
Parent Education	Positive
Higher Education Intent	Positive
Internet Access	Positive
Absences	Negative
Previous Failures	Negative
Alcohol Consumption	Negative
Going Out Frequency	Negative






