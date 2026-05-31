EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 1
EDA Optimising NYC Taxi Operations
Assignment ID: EDA/02
Prepared by: Anshu Bhardwaj
Report Document (.pdf)
This report summarizes visualizations, analysis, results, insights, assumptions, and recommendations from the NYC
Yellow Taxi 2023 exploratory data analysis notebook.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 2
1. Executive Summary
This analysis uses 2023 NYC Yellow Taxi trip records to identify demand patterns, revenue drivers, customer behavior,
payment preferences, geographic hotspots, slow operating routes, and pricing opportunities. The goal is to support an
upcoming taxi operator in improving service efficiency, maximizing revenue, and enhancing passenger experience.
Key outcomes:
- Taxi demand is concentrated in Manhattan, with Queens ranking second because of JFK and LaGuardia airport traffic.
- Demand is strongest during evening commute hours, especially around 5 PM to 7 PM.
- May and October show the strongest trip and revenue patterns.
- Most trips are short urban rides, mainly between 1 and 3 miles.
- Credit card is the dominant payment method and generates nearly all recorded tip revenue.
- Strategic cab positioning near Midtown, Times Square, Penn Station, Upper East Side, JFK, and LaGuardia can improve
utilization.
The recommendations focus on routing and dispatch, cab positioning, and pricing strategies aligned with the assignment
objectives.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 3
2. Problem Statement and Analysis Approach
NYC taxis operate in a dynamic environment where demand changes by hour, day, month, borough, pickup zone, trip
distance, payment behavior, and customer type. Taxi companies must continuously adapt their operations to meet
demand, reduce idle time, improve dispatch efficiency, and protect profitability.
The analysis approach consisted of six steps: (1) combining monthly parquet files, (2) sampling records from each month,
(3) cleaning missing values and outliers, (4) creating derived variables, (5) performing general and detailed EDA, and (6)
translating findings into operational recommendations.
Dataset used: 12 monthly 2023 NYC Yellow Taxi parquet files plus the taxi zone shapefile. The notebook sampled 2%
from each month and produced 766,203 records before cleaning and 758,610 records after removing invalid negative total
amount rows.
Important derived metrics included pickup hour, pickup day, pickup month, weekday flag, weekend flag, trip duration,
estimated actual trips, speed in miles per hour, fare per mile, fare per mile per passenger, and tip percentage.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 4
3. Data Preparation, Cleaning, and Assumptions
Data preparation: The 12 monthly files were read, a 2% sample was taken from each, and the sampled data was
concatenated into a single dataset with 766,203 records.
Data cleaning: The notebook identified missing passenger counts, RatecodeID, store-and-forward flags, congestion
surcharge, and two versions of airport fee. It also found no duplicate rows and detected negative monetary values.
Fixing columns: The airport fee was present as both airport_fee and Airport_fee. These columns were consolidated into
one airport_fee field, and the duplicate column was dropped.
Handling missing values: passenger_count was filled using median value, RatecodeID was filled with 1,
store_and_fwd_flag was filled with N, congestion_surcharge was filled with 0, and airport_fee was filled with 0. After
cleaning, all columns had zero missing values.
Handling outliers: Negative total_amount records were removed. Extreme distance outliers were detected; the maximum
observed trip distance was 33,496.83 miles, which is not plausible for an NYC taxi trip. For distance visualization, the 99th
percentile cutoff of 20.21 miles was used to show meaningful distributions.
Assumptions:
1. Missing airport fee values represent trips without an airport fee and were treated as 0.
2. Missing passenger count was imputed using median because most taxi trips are single-passenger rides.
3. Negative total amounts represent invalid or reversed transactions and were excluded for core analysis.
4. A 2% monthly sample was assumed to preserve overall seasonal and temporal patterns.
5. Cash tips are under-reported because cash gratuities are often not captured electronically.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 5
4. Demand Patterns by Month, Hour, and Day
The first stage of general EDA focused on when taxis are used most. This helps operators decide how many drivers and
vehicles should be available at different times.
Monthly insight: Demand remains relatively stable across the year, with May and October standing out as high-demand months.
February is lowest, likely due to fewer days and seasonal behavior.
Hourly insight: Taxi demand is lowest between 3 AM and 5 AM. It rises sharply after 6 AM and reaches the highest level during evening
commute hours, around 5 PM to 7 PM.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 6
Demand Patterns Continued
Weekly insight: Thursday records the highest number of trips, while Monday and Sunday are comparatively lower. Demand remains
strong across both weekdays and weekends.
Operational implication: Fleet scheduling should increase driver availability during weekday evening peaks and maintain
strong coverage from Wednesday through Saturday.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 7
5. Revenue Analysis
Revenue analysis helps determine whether trip volume translates into financial performance and which months or periods
deserve operational focus.
Monthly revenue insight: October produced the highest revenue, followed by May, December, June, and November. February had the
lowest monthly revenue. Revenue patterns closely follow demand patterns.
Highest revenue months from the notebook:
Month Total Revenue ($)
October 2,069,082.52
May 2,051,588.41
December 1,945,062.27
June 1,940,222.23
November 1,929,067.29
March 1,904,031.12
Business insight: Operators should plan for higher vehicle availability and driver coverage in October and May, while
using February for maintenance, training, and lower-cost operations.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 8
6. Payment and Tip Analysis
Payment type analysis is important because it affects customer convenience, driver earnings, and operational
transparency.
Payment insight: Credit card is the dominant payment method, followed by cash. No-charge and dispute transactions account for a very
small share of trips.
Average tip by payment type:
Payment Type Average Tip ($) Interpretation
Credit Card 4.40 Highest recorded tips
Cash 0.00 Cash tips generally not captured
Dispute 0.02 Negligible
No Charge 0.01 Negligible
Business insight: Reliable digital payment infrastructure is critical. Encouraging card payments can improve customer
convenience and increase recorded gratuity transparency for drivers.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 9
7. Passenger Behavior and Trip Distance
Passenger insight: Single-passenger trips dominate the dataset. Demand declines significantly as passenger count increases, showing
that NYC yellow taxi demand is mainly individual urban mobility.
Distance insight: Most taxi trips are short, concentrated between 1 and 3 miles. Long-distance trips are comparatively rare. The
distribution is right-skewed, which is typical for urban trips.
Business implication: Fleet positioning should prioritize dense urban zones where short trips repeat frequently. Short
rides can support high vehicle utilization when dispatching is efficient.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 10
8. Outlier Detection and Interpretation
The raw trip distance distribution was distorted by extreme outliers. The maximum distance of 33,496.83 miles is not
plausible for a taxi trip and caused normal short-distance trips to appear compressed in the histogram.
The analysis therefore used a 99th percentile distance cutoff of 20.21 miles for visualization and interpretation. This did not
mean all data had to be removed; it allowed the main business pattern to be visible.
Why it matters: Outliers can hide the real business pattern. Without treating or filtering outliers, the visual conclusions
would be misleading.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 11
9. Pickup and Dropoff Zone Analysis
Zone analysis connects trip records to actual NYC neighborhoods and transportation hubs. This is one of the strongest
business sections because it explains where drivers should be positioned.
Top pickup zones:
Zone Borough Trip Count
JFK Airport Queens 39,192
Upper East Side South Manhattan 35,831
Midtown Center Manhattan 35,033
Upper East Side North Manhattan 31,903
Midtown East Manhattan 26,953
LaGuardia Airport Queens 25,811
Penn Station/Madison Sq West Manhattan 25,729
Times Sq/Theatre District Manhattan 25,386
Top dropoff zones:
Zone Borough Trip Count
Upper East Side North Manhattan 33,095
Upper East Side South Manhattan 31,858
Midtown Center Manhattan 29,141
Times Sq/Theatre District Manhattan 23,166
Murray Hill Manhattan 22,322
Midtown East Manhattan 21,554
Lincoln Square East Manhattan 21,110
Upper West Side South Manhattan 20,768
Insight: Manhattan dominates both pickup and dropoff activity. JFK and LaGuardia are major origin points, while Midtown
and Upper East Side zones are persistent high-demand destinations.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 12
10. Borough-Level Demand and Revenue
Demand insight: Manhattan produces the majority of taxi trips, followed by Queens. The remaining boroughs contribute much smaller
volumes.
Revenue insight: Manhattan also generates the largest share of revenue. Queens is second due to airport activity from JFK and
LaGuardia.
Business implication: Vehicle allocation should prioritize Manhattan and Queens, especially Midtown, Times Square,
Upper East Side, JFK, and LaGuardia corridors.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 13
11. Detailed EDA: Routing, Traffic, and Pricing Metrics
Slow routes: The notebook calculated average route speed using trip distance and trip duration. Routes with low average
speed may indicate congestion or operational bottlenecks. These routes should be monitored for dispatch optimization.
Scaled trips: Because the analysis used a 2% sample, hourly sample trip counts were divided by 0.02 to estimate actual
hourly trips. This scaling showed the highest estimated trip volume during evening hours.
Weekday vs weekend traffic: Weekday demand is stronger during commuter hours, while weekend demand is more
spread out and can remain active later in the evening.
Night traffic: Night pickup activity is concentrated in East Village, West Village, Lower East Side, Times Square, Penn
Station, East Chelsea, and JFK Airport. Late-night operations should focus on nightlife and airport corridors.
Day vs night revenue: Daytime trips contribute most revenue, but nighttime remains strategically important because it
includes airport and entertainment travel.
Fare per mile metrics: Fare per mile was analyzed by passenger count, hour, weekday, vendor, and distance tier. These
metrics help compare profitability across different trip types and time periods.
Tip percentage: Credit card trips show much higher recorded tip percentages, while cash tips are not reliably captured.
Extra charges: Extra charges such as airport fee, tolls, congestion surcharge, MTA tax, and improvement surcharge help
explain differences between fare amount and total amount.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 14
12. Important Driver Variables Identified
Driver Variable Why It Matters Business Use
Pickup hour Demand changes sharply across the day Driver scheduling and surge planning
Pickup zone Demand is spatially concentrated Cab positioning
Borough Manhattan and Queens dominate trips and revenueFleet allocation
Trip distance Most trips are short urban rides Dispatch and pricing model
Payment type Credit card dominates and drives recorded tips Payment strategy
Trip duration / speed Identifies congestion and slow routes Routing optimization
Passenger count Most trips are solo riders Fleet and service design
The most important business drivers are pickup location, hour of day, borough, trip distance, payment type, and route
speed. Together these variables explain the strongest operational patterns in the NYC yellow taxi data.
These variables were used to derive actionable metrics such as estimated actual trips, fare per mile, fare per mile per
passenger, average route speed, pickup/dropoff ratio, revenue share, and tip percentage.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 15
13. Final Recommendations
1. Optimize routing and dispatching.
Use slow route analysis to identify routes with low average speeds and redirect drivers around bottlenecks where possible.
Dispatch systems should prioritize nearby drivers for short-distance trips to reduce empty travel time.
2. Position cabs strategically.
Concentrate vehicles in Manhattan, especially Midtown Center, Times Square, Upper East Side, Penn Station, and Murray
Hill. Maintain strong airport coverage at JFK and LaGuardia because Queens demand is heavily driven by these locations.
3. Align staffing with demand cycles.
Increase driver availability during evening peak hours from 5 PM to 8 PM. Maintain strong weekday coverage and targeted
late-night coverage in nightlife zones and airports.
4. Improve digital payment reliability.
Credit cards dominate payment behavior and generate the highest recorded tips. Reliable card payment systems directly
support customer satisfaction and driver earnings.
5. Use pricing analytics.
Monitor fare per mile by hour, day, vendor, and distance tier. This helps detect pricing inefficiencies and supports
data-driven pricing adjustments while keeping rates competitive.
6. Plan for seasonal and monthly peaks.
October and May are strong demand and revenue months. Increase operational readiness during these periods and use
lower-demand months for maintenance and training.
7. Focus on short urban trips.
Because most trips are 1 to 3 miles, the operator should focus on fast turnaround, dense-zone coverage, and reducing
driver idle time.
EDA Optimising NYC Taxi Operations - Anshu Bhardwaj Page 16
14. Conclusion
The NYC taxi market is driven by concentrated demand in Manhattan, airport-connected demand in Queens, strong
evening peaks, and high volumes of short-distance trips. Revenue follows demand closely, with October and May showing
strong performance. Credit card payments dominate the payment mix and account for the majority of recorded gratuities.
The analysis supports clear operational strategies: place drivers where demand is strongest, schedule more drivers during
peak hours, monitor slow routes, maintain reliable payment systems, and use fare-per-mile metrics to guide pricing
decisions. These actions can reduce idle time, increase utilization, improve customer service, and strengthen revenue
performance.
This report is based on a 2% sample of 2023 NYC Yellow Taxi data and should be read with the stated assumptions
around sampling, missing values, and outlier treatment.
Appendix: Notebook-based visualizations and tables were used to prepare this report. The accompanying interactive Python notebook
contains the full code and outputs.
