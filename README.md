# MentalHealthCrisisinTech-
This analysis seeks to answer the question ‘Is there a Mental Health Crisis in the tech industry?’ 
The tech industry is known for its high-pressure environment, long hours, and rapid pace of change, which can all contribute to mental health challenges.Addressing this issue is crucial for the well-being of tech workers and the overall productivity of the industry.
The World Health Organization (WHO) estimates that nearly 264 million people globally suffer from depression and anxiety. With the growing number of individuals in the tech industry, it is essential to consider the mental health status of these workers.
Please see article here https://kiiomgps.medium.com/mental-health-in-tech-77efe16b4fb7
Following SQL codes were run to analyze the data file 
link to data file (https://www.kaggle.com/datasets/anth7310/mental-health-in-the-tech-industry)

/*look at General population of repondents surveyed*/
SELECT COUNT (*)
FROM 
survey_mental_health;

SELECT COUNT(*) AS Num_of_Respondents, year AS Year 
FROM survey_mental_health 
WHERE year IN ('2014', '2015', '2016')
GROUP BY Year
ORDER BY Year

SELECT COUNT (DISTINCT country)
FROM survey_mental_health;

SELECT country, COUNT(*)
FROM survey_mental_health
GROUP BY country;SELECT COUNT (*),gender
FROM survey_mental_health
GROUP BY gender

/* Number of tech employees in the tech industry and their work location*/

SELECT COUNT (tech_company) 
FROM survey_mental_health
WHERE tech_company = 'Yes'
AND remote_work = 'Yes';

SELECT COUNT (tech_company) 
FROM survey_mental_health
WHERE tech_company = 'Yes'
AND remote_work = 'No'

/*Run code to look at gender of repondents*/

SELECT COUNT (*),gender ,tech_company
FROM survey_mental_health
WHERE tech_company = 'Yes'
GROUP BY gender;

/* Number of non tech employees getting mental heakth treatment andt then grouping them by gender */

SELECT COUNT(*) AS count, gender, tech_company 
FROM survey_mental_health 
WHERE tech_company = 'No' 
AND treatment IN ('Yes', 'No') 
GROUP BY gender ;

/* Insights into the demographics of tech company employees, both globally and within the United States*/

SELECT MAX (age) , MIN (age) , ROUND (AVG ( age))
FROM survey_mental_health
WHERE tech_company = 'Yes';

SELECT MAX (age) , MIN (age) , ROUND (AVG ( age))
FROM survey_mental_health
WHERE tech_company = 'Yes' 
AND Country = 'United States';

/* Highlights the varying levels of mental health treatment among different gender groups*/

SELECT gender, treatment, COUNT(*) 
FROM survey_mental_health
GROUP BY gender, treatment;

/*Groups the survey respondents into age groups and count the number of respondents in each group*/

SELECT CASE
WHEN age < 20 THEN 'Under 20' 
WHEN age BETWEEN 20 AND 29 THEN '20-29' 
WHEN age BETWEEN 30 AND 39 THEN '30-39' 
WHEN age BETWEEN 40 AND 49 THEN '40-49' 
ELSE '50 and above' 
END AS age_group, 
COUNT(*) 
FROM survey_mental_health
GROUP BY age_group;

/*Highlights disparity in mental health treatment across age groups*/

SELECT treatment,
CASE 
WHEN age < 20 THEN 'Under 20' 
WHEN age BETWEEN 20 AND 29 THEN '20-29' 
WHEN age BETWEEN 30 AND 39 THEN '30-39' 
WHEN age BETWEEN 40 AND 49 THEN '40-49' 
ELSE '50 and above' 
END AS age_group,
COUNT(*) 
FROM survey_mental_health
WHERE treatment IN ('Yes', 'No')
GROUP BY age_group , treatment ;

/*Highlights trends towards treatment and effect of family history of mental illness on treatment among different age groups*/

SELECT treatment,
CASE 
WHEN age < 20 THEN 'Under 20' 
WHEN age BETWEEN 20 AND 29 THEN '20-29' 
WHEN age BETWEEN 30 AND 39 THEN '30-39' 
WHEN age BETWEEN 40 AND 49 THEN '40-49' 
ELSE '50 and above' 
END AS age_group,
CASE 
WHEN family_history = 'Yes' THEN 'High Risk for Mental illess'
WHEN family_history = 'No' THEN 'Low Risk for Mental illness'
End AS Potential_for_Mental_Illness,
COUNT(*) 
FROM survey_mental_health
WHERE treatment IN ('Yes', 'No')
GROUP BY treatment,age_group ,Potential_for_Mental_Illness
ORDER BY COUNT(*) DESC;

/* A look at employee gender proportions across various geographic regions */

SELECT gender, COUNT(*) AS count, ROUND((COUNT(*) * 100.0 / 
(SELECT COUNT(*) FROM survey_mental_health)), 2)
AS percentage 
FROM survey_mental_health GROUP BY

SELECT COUNT (tech_company) ,tech_company
FROM survey_mental_health
WHERE tech_company = 'Yes'
OR tech_company = 'No'
GROUP BY tech_company;

SELECT gender, COUNT(tech_company) AS count, ROUND((COUNT(*) * 100.0 / 
(SELECT COUNT(*) FROM survey_mental_health)), 2)
AS percentage 
FROM survey_mental_health 
GROUP BY gender;

/* Representation of female and other gender minorities in the tech industry*/

SELECT COUNT(*) As Num_employees, region,gender
FROM survey_mental_health
GROUP BY region,gender
ORDER BY region,  gender, Num_employees 

/* Window function to COUNT tech employees by regions*/

SELECT
    country,
    region,
    COUNT(*) AS record_count,
    RANK() OVER (PARTITION BY region ORDER BY COUNT(*) DESC) AS rank
FROM
    survey_mental_health
   WHERE tech_company = 'Yes'
GROUP BY
    country, region;

  /* Self employement status and meatal health treatment*/
  
SELECT self_employed ,COUNT (*) AS Num_employed ,treatment
FROM survey_mental_health 
WHERE treatment IN ('Yes','No')
GROUP BY self_employed,treatment
ORDER BY Num_employed DESC;

/*query aims to analyze the relationship between age, family history of mental illness, and the likelihood of seeking mental health treatment among individuals in the United States */

SELECT treatment,
CASE 
WHEN age < 20 THEN 'Under 20' 
WHEN age BETWEEN 20 AND 29 THEN '20-29' 
WHEN age BETWEEN 30 AND 39 THEN '30-39' 
WHEN age BETWEEN 40 AND 49 THEN '40-49' 
ELSE '50 and above' 
END AS age_group,
CASE 
WHEN family_history = 'Yes' THEN 'High Risk for Mental illess'
WHEN family_history = 'No' THEN 'Low Risk for Mental illness'
End AS Potential_for_Mental_Illness,
COUNT(*) 
FROM survey_mental_health
WHERE treatment IN ('Yes', 'No') 
AND country = 'United States'
GROUP BY treatment,age_group ,Potential_for_Mental_Illness
ORDER BY COUNT (*) DESC;

/*Which gender is likely to experience mental illness*/

SELECT gender,
CASE
WHEN family_history = 'Yes' THEN 'High Risk for Mental illess'
WHEN family_history = 'No' THEN 'Low Risk for Mental illness'
END AS Potential_for_Mental_Illness,
COUNT(*) 
FROM survey_mental_health
WHERE country ='United States'
GROUP BY gender,Potential_for_Mental_Illness
ORDER BY COUNT (*) DESC;


/* Which region is likely to experience mental illness*/

SELECT region,
CASE
WHEN family_history = 'Yes' THEN 'High Risk for Mental illess'
WHEN family_history = 'No' THEN 'Low Risk for Mental illness'
END As Potential_for_Mental_Illness,
COUNT(*) 
FROM survey_mental_health
WHERE region IN ('North America', 'Europe')
GROUP BY region,Potential_for_Mental_Illness
ORDER BY COUNT(*) DESC;

/* Distribution of mental illness among different states and which States ahd significantly higher mental illness patients*/

SELECT state,country,
CASE
WHEN family_history = 'Yes' THEN 'High Risk for Mental illess'
WHEN family_history = 'No' THEN 'Low Risk for Mental illness'
END AS Potential_for_Mental_Illness,
COUNT(*) 
FROM survey_mental_health
WHERE country ='United States'
GROUP BY state,Potential_for_Mental_Illness
ORDER BY COUNT(*) DESC;


/* Regions and Countries that had high cases of tech eomloyees seeking mental health treatment*/

SELECT country, region, 
  ROW_NUMBER() OVER (PARTITION BY region ORDER BY country) AS row_num, 
  RANK() OVER (PARTITION BY region ORDER BY country) AS rank,
  DENSE_RANK() OVER (PARTITION BY region ORDER BY country) AS dense_rank 
  FROM survey_mental_health 
  WHERE tech_company = 'Yes' 
  GROUP BY region, country;
  
SELECT 
    region,
    country,
    COUNT(treatment) AS total_count,
    ROW_NUMBER() OVER (PARTITION BY region_ ORDER BY COUNT(treatment) 
DESC) AS row_number
FROM 
    survey_mental_health
WHERE 
    treatment = 'Yes'
GROUP BY 
    region, country
ORDER BY 
    region, row_number;

/* comprehensive analysis of mental health treatments across different regions and countries, including detailed statistics on treatment counts, percentage distributions, average age of respondents, and gender distribution*/

SELECT 
    region,
    country,
    COUNT(treatment) AS total_treatments,
    (COUNT(treatment) * 100.0 / SUM(COUNT(treatment)) 
OVER (PARTITION BY region_)) AS percentage_of_treatments,
    AVG(age) AS average_age,
    SUM(CASE WHEN gender = 'Male' THEN 1 ELSE 0 END) AS male_count,
    SUM(CASE WHEN gender = 'Female' THEN 1 ELSE 0 END) AS female_count,
    SUM(CASE WHEN gender NOT IN ('Male', 'Female') THEN 1 ELSE 0 END) 
AS other_count
FROM 
    survey_mental_health
WHERE 
    treatment = 'Yes'
GROUP BY 
    region, country
ORDER BY 
    region, total_treatments DESC;


Conclusion
This survey indeed shines a spotlight on the significant mental health challenges faced by tech workers. The data provides a wealth of insights that can inform strategies to address these issues effectively. The findings emphasize the urgency of prioritizing mental health support within the tech industry. Further analysis of this data could uncover even deeper insights, helping to tailor interventions and support systems that can make a real difference. The value of this survey cannot be overstated — it has the potential to drive meaningful change and improve the well-being of tech professionals.

The analysis of mental health within various countries, particularly among employees in the tech industry, has revealed significant findings. The survey underscores the growing recognition of mental health issues and the critical importance of accessible and effective treatment options.

Companies and healthcare providers must continue to prioritize mental health support to ensure that employees can maintain both their well-being and productivity. Overall, these insights highlight the need for ongoing efforts to address mental health challenges in the workplace and beyond.
   
