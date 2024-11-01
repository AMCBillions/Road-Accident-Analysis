# Road-Accident-Analysis
![Road Accident](https://github.com/user-attachments/assets/c7b2c921-5add-4880-b575-9c196ab647ba)

**SUMMARY**:
This report is based on an analysis of a Dataset downloaded from Kaggle. The Dataset is about a collection of road accident data for a period of two years, i.e.; 2021 to 2022 in England. The purpose of the analysis is to derive some insights based on certain requirements as outlined below. This report is a comparison of results earlier obtained from a dashboard created in Power BI earlier, which can be referred to (yet to be shared). This analysis is performed in SQL (SQL Server Management Studio).

**REQUIREMENTS**:
How do different conditions affect road accidents. These conditions are; road lighting, type of road (single carriage or dual), wet or dry etc.

 

**TOOLS USED**:
Microsoft Power BI and SQL (Microsoft SQL Server).

**ASSUMPTIONS**:
It is assumed that the audience understands how Microsoft SQL Server Management Studio environment is setup. 

## REPORT

**METHODOLGY**: Queries will be fired in SQL for each specific requirement and the result pasted under the query as a way of validating the output with Power BI.

 **SQL QUERIES/RESULTS**

1.       Primary KPIs without filters for other conditions

i)                    Current Year Total Casualties (CY Total Casualties) – 2022

SELECT SUM (number_of_casualties) AS CY_Casualties

FROM road_accident

WHERE YEAR (accident_date) = '2022' 

Output: 

![Output1](https://github.com/user-attachments/assets/38732a58-a20f-4826-94fc-bc195bd76e0d)



ii)                   Current Year Total Accidents - 2022

SELECT DISTINCT (COUNT (accident_index)) AS CY_Accidents

FROM road_accident

WHERE YEAR (accident_date) = '2022'  

Output: 

![Output2](https://github.com/user-attachments/assets/24b3d540-4d3c-4c39-ba21-e54cdfca1771)



 iii)    Current Year Fatal Accidents

SELECT

SUM (number_of_casualties)

FROM road_accident

WHERE YEAR (accident_date) = '2022' AND accident_severity = 'Fatal' 

Output: 

![Output3](https://github.com/user-attachments/assets/e1e2d087-8b0a-4dd9-ad2f-92adaddfa922)



iv)      Current Year Serious Casualties

SELECT

SUM (number_of_casualties)

FROM road_accident

WHERE YEAR (accident_date) = '2022' AND accident_severity = 'Serious'

Output: 

![Output4](https://github.com/user-attachments/assets/f7931988-e510-4e1b-9259-ab5c40ae8c29)



 v)       Current Year Slight Casualties

SELECT

SUM (number_of_casualties)

FROM road_accident

WHERE YEAR (accident_date) = '2022' AND accident_severity = 'Slight'

Output: 

![Output5](https://github.com/user-attachments/assets/b8bd83f8-8fac-49d6-bb02-229010b28113)



 vi)      Current Year Casualties by Vehicle Type 

SELECT

   CASE

          WHEN vehicle_type IN ('Agricultural vehicle') THEN 'Agricultural'

          WHEN vehicle_type IN ('Car','Taxi/Private hire car') THEN 'Cars'

          WHEN vehicle_type IN ('Motorcycle 125cc and under','Motorcycle 50cc and under', 'Motorcycle over 125cc and up to 500cc','Motorcyle over 500cc','Pedal cycle','Ridden horse') THEN 'Bikes'

          WHEN vehicle_type IN ('Bus or coach (17 or more pass seats)','Minibus (8 - 16 passenger seats)') THEN 'Buses'

          WHEN vehicle_type IN ('Goods 7.5 tonnes mgw and over','Goods over 3.5t. and under 7.5t','Van / Goods 3.5 tonnes mgw or under') THEN 'Trucks'

              ELSE 'Others'

       END As vehicle_group,

       SUM (number_of_casualties) as CY_Casualties

       FROM dbo.road_accident

       WHERE YEAR (accident_date) = '2022'

       GROUP BY

              CASE

              WHEN vehicle_type IN ('Agricultural vehicle') THEN 'Agricultural'

              WHEN vehicle_type IN ('Car','Taxi/Private hire car') THEN 'Cars'

              WHEN vehicle_type IN ('Motorcycle 125cc and under','Motorcycle 50cc and under', 'Motorcycle over 125cc and up to 500cc','Motorcyle over 500cc','Pedal cycle','Ridden horse') THEN 'Bikes'

              WHEN vehicle_type IN ('Bus or coach (17 or more pass seats)','Minibus (8 - 16 passenger seats)') THEN 'Buses'

              WHEN vehicle_type IN ('Goods 7.5 tonnes mgw and over','Goods over 3.5t. and under 7.5t','Van / Goods 3.5 tonnes mgw or under') THEN 'Trucks'

              ELSE 'Others'

       END 

Output: 

![Output6](https://github.com/user-attachments/assets/582cf421-7f2a-4ba2-822c-091635f85a41)



 vii)    Casualties by Month by Year      

       SELECT DATENAME (MONTH, accident_date) AS Month_Name,

       SUM (number_of_casualties) AS CY_Casualties

       FROM road_accident

       WHERE YEAR (accident_date) = '2022'

       GROUP BY DATENAME (MONTH, accident_date)

         Output:

![Output7](https://github.com/user-attachments/assets/b88ffc74-7b8d-4363-b13c-c9aed83bee34)



             

       SELECT DATENAME (MONTH, accident_date) AS Month_Name,

       SUM (number_of_casualties) AS PY_Casualties

       FROM road_accident

       WHERE YEAR (accident_date) = '2021'

       GROUP BY DATENAME (MONTH, accident_date) 

          Output:

![Output8](https://github.com/user-attachments/assets/021b4ee3-bdcc-4263-a505-fe9cd95f2bc8)



   

 

 

viii)  Casualties by Road Type by Year        

       SELECT road_type,

       SUM (number_of_casualties) as PY_Casualties

       FROM dbo.road_accident

       WHERE YEAR (accident_date) = '2021'

       GROUP BY road_type

       Output:

![Output9](https://github.com/user-attachments/assets/a8761d61-722c-4477-b54f-85eb97fd0da2)



       

       SELECT road_type,

       SUM (number_of_casualties) as CY_Casualties

       FROM dbo.road_accident

       WHERE YEAR (accident_date) = '2022'

       GROUP BY road_type 

 

Output: 

![Output10](https://github.com/user-attachments/assets/4b515dcf-7a5d-4a04-8975-b42e76d749a8)



   

 

ix)    Casualties by Urban/Rural 

        SELECT

              CASE

              WHEN urban_or_rural_area IN (‘Urban') THEN 'Urban'

              ELSE 'Rural'

       END AS Area_Type,

       SUM (number_of_casualties) as PY_Casualties

       FROM dbo.road_accident

       WHERE YEAR (accident_date) = '2021'

       GROUP BY

              CASE

              WHEN urban_or_rural_area IN (‘Urban') THEN 'Urban'

              ELSE 'Rural'

       END

                    Output: 

![Output11](https://github.com/user-attachments/assets/96bc04db-cfa1-41fc-a0c5-b3659f8c32c1)


  

 

x)   Percentage Casualties by Urban/Rural

      

SELECT

urban_or_rural_area, CAST (SUM (number_of_casualties) AS DEC (10,2)) *100/

(SELECT CAST (SUM (number_of_casualties) AS DEC (10,2)) FROM road_accident WHERE YEar(accident_date) = '2022')

AS Area_Percent

 FROM road_accident

WHERE YEAR (accident_date) = '2022'

GROUP BY urban_or_rural_area 

       Output: 

![Output12](https://github.com/user-attachments/assets/07ab964f-83e9-4402-be73-c51475ff3334)


     

 

xi) Casualties by Lighting conditions 

SELECT

       CASE

       WHEN light_conditions IN ('Darkness - lighting unknown','Darkness - lights lit','

       Darkness - lights unlit','Darkness - no lighting') THEN 'Night'

       ELSE 'Day'

       END AS lighting_group,

       CAST (CAST (SUM (number_of_casualties) AS DEC (10,2)) * 100/

       (SELECT CAST (SUM (number_of_casualties) AS DEC (10,2)) FROM road_accident WHERE YEAR (accident_date) = '2022') AS DEC (10,2))

       AS Percent_lighting

       FROM dbo.road_accident

       WHERE YEAR (accident_date) = '2022'

       GROUP BY

       CASE

       WHEN light_conditions IN ('Darkness - lighting unknown','Darkness - lights lit','

       Darkness - lights unlit','Darkness - no lighting') THEN 'Night'

       ELSE 'Day'

       END      

       Output: 

![Output13](https://github.com/user-attachments/assets/f3f829f5-62e1-4b1d-b897-c21968530c57)



  

 

xii) Casualty count by top ten Cities/Towns

 SELECT

TOP 10 local_authority, SUM (number_of_casualties) AS Total_Casualties

FROM

dbo.road_accident

GROUP BY

local_authority

ORDER BY Total_Casualties DESC 

Output: 

![Output14](https://github.com/user-attachments/assets/0b20948e-8902-44e2-a260-92c61f2e3fc6)

**Conclusion**:

It is important to observe from the primary KPI’s that there is a negative percentage increase on current year (CY) on these classes; Casualties, Accidents, Fatal Casualties, Serious Casualties and Slight Casualties. These are better seen from Power BI visuals (to be availed later). Previous year (PY) against Current Year (CY) shows a month to month decrease for year 2022. Reasons could be that a review was done for 2021 and few measures as per recommendations implemented. We also note/observe that casualties by vehicle type are more on cars . Single carriage roads (expectedly) have the highest casualties at 145K, with an observation that urban areas also stand at 61.95% casualties compared to 38.05% for rural. When it comes to lighting conditions, there are more casualties in the day (73.84%) than in the night (26.16%), expectedly so because people tend to move/drive more in the day.

One of the recommendations I would make is to increase dual carriages in those areas with single carriages if the funds and terrain allow. A further investigation should be done as to why cars (sedans) tend to have more casualties. A recommendation could speed limits as well as a further investigation into driver behaviour(random cameras that are able to capture the driver). 

**Power BI Dashboard**

