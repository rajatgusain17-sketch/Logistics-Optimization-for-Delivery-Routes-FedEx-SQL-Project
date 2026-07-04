# FEDEX-SQL-PROJECT  
## Project Overview
FedEx, one of the world’s largest logistics and courier service providers, operates an extensive global air and ground transportation network, connecting more than 220 countries and territories through major hubs in Memphis, Dubai, Singapore, Paris, and London.   
The network includes international sortation centers, regional hubs, and last-mile delivery partners that handle millions of shipments daily across continents.
As global e-commerce volumes continue to surge — especially during peak seasons and holidays — flight delays, customs bottlenecks, and last-mile congestion significantly impact both on-time delivery performance and operational costs.  

### Challenges faced by FEDEX     

Currently, FedEx’s logistics operations face challenges in:   
● Identifying the root causes of transit and delivery delays (e.g., customs clearance, weather disruptions, route congestion).       
● Optimizing international and regional routes for faster, more cost-effective deliveries.          
● Improving shipment efficiency, warehouse throughput, and agent performance using data-driven insights.         
The logistics data, stored in relational databases, can be analyzed using SQL to extract
meaningful performance metrics and delay patterns.       
These insights can help FedEx improve hub utilization, route planning, and service-level
reliability across its international network.      

---

## Objective           
Build a SQL-driven logistics analytics system to analyze shipment performance, optimize routes, and enhance delivery efficiency across FedEx’s global network.
The project aims to:   
● Identify delay patterns and operational inefficiencies.      
● Optimize hub and route combinations for improved transit times.         
● Analyze agent- and warehouse-level performance.         
● Recommend actionable improvements that strengthen on-time delivery and cost efficiency.         

---

## Dataset Used     
The dataset will include the following key tables:
### 1. Orders Table
The Orders dataset contains order-level delivery details including order date, route, warehouse,and payment type.    
#### Columns:
Order_ID, Customer_ID, Order_Date, Route_ID, Warehouse_ID, Order_Amount,Delivery_Type, Payment_Mode
### 2. Routes Table
The Routes dataset includes route-level transportation details, covering the source and destination cities, countries, total distance, and average transit time (in hours).
#### Columns:   
Route_ID, Source_City, Source_Country, Destination_City, Destination_Country, Distance_KM, Avg_Transit_Time_Hours
### 3. Warehouses Table
The Warehouses dataset provides location-level information about FedEx’s major hubs and sortation centers.
#### Columns:      
Warehouse_ID, City, Country, Capacity_per_day, Manager_Name
### 4. Delivery Agents Table
The Delivery Agents dataset contains agent-level performance data, including agent ID, name, assigned zone and country, experience, and customer rating.
#### Columns:      
Agent_ID, Agent_Name, Zone, Zone_Country, Experience_Years, Avg_Rating
### 5. Shipment Tracking Table
The Shipments dataset includes shipment-level tracking information with timestamps for pickup and delivery, delay duration, and feedback ratings. It captures operational data that helps analyze transit time, on-time delivery percentage, and service-level adherence across routes and agents.
#### Columns:       
Shipment_ID, Order_ID, Agent_ID, Route_ID, Warehouse_ID, Pickup_Date, Delivery_Date, Delivery_Status, Delay_Hours, Delivery_Feedback      

--- 

## Tools and Technologies
* SOL Querying
* Power BI Visualization
* Interactive Dashboard Design

 ---
 
## Key Business Metrics and SQL Solutions
### 1. To calculate "DISTANCE-TO-TIME EFFICIENCY RATIO = DISTANCE_KM / AVG_TRANSIT_TIME_HOURS".

#### The Business Problem:      
How quickly are goods moving across our global transit lanes? Simply looking at total transit time doesn't tell the whole story—a long-distance route will naturally take more hours than a short one. To fairly evaluate route performance, we need to calculate the *effective speed (KM per Hour)* for each route. Identifying paths with an abnormally low efficiency ratio allows logistics managers to flag structural bottlenecks, complex customs checkpoints, or slow carrier segments.

*The SQL Code:*   
```SELECT Route_ID,  Source_City, Destination_City, Distance_KM, Avg_Transit_Time_Hours, ROUND(Distance_KM / Avg_Transit_Time_Hours, 2) AS Efficiency_Ratio_KMH FROM fedex_routes ORDER BY Efficiency_Ratio_KMH DESC;```
#### Key Insight :
​Top Performers: Long-haul air channels like Johannesburg to Dubai (R019) operate at peak efficiency with a velocity of 545.49 KM/H, closely followed by Los Angeles to Amsterdam (R010) at 540.34 KM/H. This indicates highly optimized flight paths and rapid customs clearance.
#### Screenshot:  ![dstance to time eff. ratio](https://github.com/rajatgusain17-sketch/Logistics-Optimization-for-Delivery-Routes-FedEx-SQL-Project/blob/main/Distance%20to%20Time%20eff%20ratio.png?raw=true)

---

### 2. To calculate AVERAGE DELIVERY DELAY PER SOURCE COUNTRY.
#### The Business Problem:     
Which global origin hubs are introducing the most friction into our supply chain? Total shipments don't tell the full story—a country might handle high volume efficiently while a smaller hub causes severe delivery backlogs. To isolate systemic bottlenecks at the point of origin, we need to calculate the *Average Delay Hours* across different source countries. This metric allows logistics operations to pinpoint specific international custom choke points, localized warehouse inefficiencies, or carrier delays.

*The SQL Code:*            
``` SELECT r.Source_Country, COUNT(s.Shipment_ID) AS Total_Shipments, ROUND(AVG(s.Delay_Hours), 2) AS Avg_Delay_Hours  FROM  fedex_shipments s  JOIN  fedex_routes r ON s.Route_ID = r.Route_ID  GROUP BY   r.Source_Country  ORDER BY  Avg_Delay_Hours DESC;```
#### Key Insight :
The High-Impact Risk: China stands out as a critical operational vulnerability. It processes a massive volume of 95 shipments while suffering a severe average delay of 32.53 hours.    
​The Peak Outlier: The UAE records the highest pure friction with an average delay of 41.04 hours over 47 shipments.                
The Baseline Efficiency: On the other end of the spectrum, Western hubs like the USA (16.95 hours over 144 shipments) and Canada (9.72 hours over 38 shipments) demonstrate high efficiency and fast dispatch pipelines despite processing higher overall volume.      
#### Screenshot: ![average delivery delay](https://github.com/rajatgusain17-sketch/FEDEX-SQL-PROJECT/blob/main/Avg%20delivery%20delay.png?raw=true)

---

### 3. To calculate On Time Delivery %(OTD).
#### The Business Problem:
To evaluate the operational excellence of our global warehouses, we must calculate the On-Time Delivery (OTD) Percentage. This KPI helps management identify which facilities are meeting customer expectations and which are becoming bottlenecks, allowing for targeted resource allocation.      

*The SQL Code:*             
```SELECT  w.Warehouse_ID,w.city, COUNT(s.Shipment_ID) AS Total_Deliveries, ROUND(SUM(CASE WHEN s.Delay_Hours = 0 THEN 1 ELSE 0 END) * 100.0 / COUNT(s.Shipment_ID), 2)```
```AS Warehouse_OTD_Percentage  FROM fedex_shipments s ```
```JOIN fedex_warehouses w ON s.Warehouse_ID = w.Warehouse_ID```
```GROUP BY w.Warehouse_ID, w.city;``` 

#### Key Insights:
Top Performer: Amsterdam leads the network with an OTD percentage of 10.71%.     
​Network Reality: The overall network average OTD% sits at 6.72%. This metric serves as a baseline, indicating that current operational processes require significant optimization to improve reliability across all regions.             
#### Screenshot: ![on time delivery %](https://github.com/rajatgusain17-sketch/FEDEX-SQL-PROJECT/blob/main/on%20timedelivery%20%25.png?raw=true)

---

### 4. To calcuate  AVERAGE DELAY (IN HOURS) PER ROUTE_ID.
#### The Business Problem:     
To optimize the supply chain, we must move beyond general averages and identify which specific transit paths are underperforming. By analyzing the Average Delay per Route_ID, we can isolate systemic operational bottlenecks, such as slow customs clearance or poor infrastructure on specific paths, allowing management to prioritize targeted improvements.           
*The SQL Code:*      
```SELECT  Route_ID, ROUND(AVG(Delay_Hours), 2) AS Avg_Delay_Hours, COUNT(Shipment_ID) AS Total_Shipments```
```FROM fedex_shipments   GROUP BY Route_ID  ORDER BY Avg_Delay_Hours DESC;```  
#### Key Insight:      
Network Performance Baseline: Across the analyzed dataset, the global average delay is 25.12 hours calculated from a total of 1,034 shipments.
​Operational Outliers: Route R002 is the most significant bottleneck, exhibiting an average delay of 41.04 hours, which is substantially higher than the network average.
​Actionable Intelligence: Routes such as R002 and R007 (34.01 hours) are immediate candidates for a root-cause investigation, as they are dragging down the overall efficiency of the network.            
#### Screenshot: ![average delay per route id](https://github.com/rajatgusain17-sketch/FEDEX-SQL-PROJECT/blob/main/Avg%20delapy%20per%20route%20id.png?raw=true)  


 ---


## Key Insights & Business Recommendations

### Core Analytical Insights
* *The "On-Time" Crisis*: The network is currently failing to meet delivery promises, with an overall Average OTD% of 6.72% and top performers like Amsterdam only reaching 10.71%.
* *High-Friction Regions*: UAE (41.04 hrs) and China (32.53 hrs) are driving the highest average delays; China is a significant bottleneck due to its high volume of 95 shipments.
* *Route Specificity*: A massive performance gap exists between Route R001 (6.83 hrs) and Route R002 (41.04 hrs), suggesting location-specific transit or customs issues.
* *Utilization Paradox*: Expensive facilities in hubs like Toronto and Johannesburg are sitting nearly empty, with utilization rates as low as 0.20% to 0.30%.

### Strategic Recommendations
* *Supply Chain Audit*: Investigate Customs Clearance and Carrier Partnerships in the UAE and China.
* *SLA Realignment*: Adjust "Estimated Delivery Date" expectations to reflect the real-world 25-hour average delay to improve customer trust.
* *Route R002 Deep Dive*: Model the logistics flow of R002 after the high-performing R001 route to reduce delays.
* *Optimize Warehouse Footprint*: Consolidate under-utilized hubs using a "Hub-Spoke" model to reduce fixed operational costs.

  ---


## ​Learning Outcomes   
* #### Data-Driven Problem Solving:                          
Learned to translate complex, messy logistics datasets into clear performance metrics, specifically identifying a systemic "on-time" delivery crisis where OTD rates were under 10%.           
* #### Root Cause Analysis (RCA):                
Gained proficiency in isolating specific bottlenecks by comparing performance across regions and routes, such as identifying why Route R002 experienced a 41-hour delay while R001 remained highly efficient.              
* #### Operational Strategy:      
Developed the ability to propose strategic business changes based on quantitative evidence, such as recommending a "Hub-Spoke" consolidation model to address extreme warehouse under-utilization (0.20%–0.30%).              
* #### Advanced Analytical Modeling:     
Learned to uncover latent operational patterns, such as the "Tipping Point" where warehouse utilization over 85% causes exponential dispatch delays.                
* #### Evaluating Business Infrastructure:                   
Enhanced skills in assessing Service Level Agreements (SLAs) by uncovering "SLA Erosion," where premium-paying customers were receiving standard-level service.         
* #### Contextual Data Interpretation:            
Learned how to apply the "80/20 Rule" to logistics to focus on the most impactful issues, such as specific mechanical and traffic failures that drive the majority of extreme delay outliers.                

---

## Conclusion
​This project successfully utilized SQL to uncover critical bottlenecks in the FedEx logistics network, highlighting issues such as a low on-time delivery rate and severe warehouse under-utilization. By identifying these inefficiencies and recommending strategic actions—including SLA realignment, facility consolidation, and route modeling—this analysis provides a clear, data-driven roadmap for operational improvement.    
Ultimately, this project demonstrates how data analysis can be used to reduce costs and improve service reliability in complex supply chain environments.
