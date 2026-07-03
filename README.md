# FEDEX-SQL-PROJECT  
## Project Overview
FedEx, one of the world’s largest logistics and courier service providers, operates an extensive global air and ground transportation network, connecting more than 220 countries and territories through major hubs in Memphis, Dubai, Singapore, Paris, and London.   
The network includes international sortation centers, regional hubs, and last-mile delivery partners that handle millions of shipments daily across continents.
As global e-commerce volumes continue to surge — especially during peak seasons and holidays — flight delays, customs bottlenecks, and last-mile congestion significantly impact both on-time delivery performance and operational costs.  

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
### 1.The Question: 
To calculate "DISTANCE-TO-TIME EFFICIENCY RATIO = DISTANCE_KM / AVG_TRANSIT_TIME_HOURS".
Metric: Distance-to-Time Efficiency Ratio (Route Velocity Analytics)

*The Business Problem:* How quickly are goods moving across our global transit lanes? Simply looking at total transit time doesn't tell the whole story—a long-distance route will naturally take more hours than a short one. To fairly evaluate route performance, we need to calculate the *effective speed (KM per Hour)* for each route. Identifying paths with an abnormally low efficiency ratio allows logistics managers to flag structural bottlenecks, complex customs checkpoints, or slow carrier segments.

*The SQL Solution:*
```SELECT Route_ID,  Source_City, Destination_City, Distance_KM, Avg_Transit_Time_Hours, ROUND(Distance_KM / Avg_Transit_Time_Hours, 2) AS Efficiency_Ratio_KMH FROM fedex_routes ORDER BY Efficiency_Ratio_KMH DESC;```
Key Insight from Visual Output:
​The data exposes a massive performance delta across the network:
​Top Performers: Long-haul air channels like Johannesburg to Dubai (R019) operate at peak efficiency with a velocity of 545.49 KM/H, closely followed by Los Angeles to Amsterdam (R010) at 540.34 KM/H. This indicates highly optimized flight paths and rapid customs clearance.

Screenshot: ![dstance to time eff. ratio]()







---


## Screenshots
### 1. Average Delay(in hours) per Route ID:![average delay per route id](https://github.com/rajatgusain17-sketch/FEDEX-SQL-PROJECT/blob/main/Avg%20delapy%20per%20route%20id.png?raw=true)   
### 2. On Time Delivery %:![on time delivery %](https://github.com/rajatgusain17-sketch/FEDEX-SQL-PROJECT/blob/main/on%20timedelivery%20%25.png?raw=true)
### 3. Average Delivery Delay per Source Country:![average delivery delay](https://github.com/rajatgusain17-sketch/FEDEX-SQL-PROJECT/blob/main/Avg%20delivery%20delay.png?raw=true)
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
