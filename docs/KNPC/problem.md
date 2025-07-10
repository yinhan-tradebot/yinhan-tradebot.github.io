# **Tank Maintenance Scheduling: Problem Specification**  

## **1. Objective**  

Develop an optimization model to schedule tank maintenance while adhering to operational constraints, ensuring continuous product availability.  

## **2. Data**  

| Site         | Tank ID    | Product         | Capacity (Million Litres) | Maintenance Duration (Months) |  
|--------------|------------|-----------------|--------------------------|-------------------------------|  
| Sabhan Depot | TK-SD-01   | Premium         | 18.0                     | 12                            |  
| Sabhan Depot | TK-SD-02   | Premium         | 18.0                     | 12                            |  
| Sabhan Depot | TK-SD-03   | Super Premium   | 18.0                     | 12                            |  
| Sabhan Depot | TK-SD-04   | Super Premium   | 4.5                      | 8                             |  
| Sabhan Depot | TK-SD-05   | Premium         | 4.5                      | 8                             |  
| Sabhan Depot | TK-SD-06   | Gas Oil         | 4.5                      | 8                             |  
| Sabhan Depot | TK-SD-07   | Gas Oil         | 8.464                    | 10                            |  
| Sabhan Depot | TK-SD-08   | Gas Oil         | 8.464                    | 10                            |  
| Sabhan Depot | TK-SD-09   | Water           | 1.25                     | 6                             |  
| Sabhan Depot | TK-SD-10   | Water           | 7.5                      | 10                            |  
| Ahmadi Depot | TK-AD-01   | Premium         | 18.0                     | 12                            |  
| Ahmadi Depot | TK-AD-02   | Super Premium   | 18.0                     | 12                            |  
| Ahmadi Depot | TK-AD-03   | Premium         | 4.5                      | 8                             |  
| Ahmadi Depot | TK-AD-04   | UL 98           | 4.5                      | 8                             |  
| Ahmadi Depot | TK-AD-05   | UL 98           | 1.5                      | 6                             |  
| Ahmadi Depot | TK-AD-06   | Kerosene        | 9.0                      | 10                            |  
| Ahmadi Depot | TK-AD-07   | Kerosene        | 9.0                      | 10                            |  
| Ahmadi Depot | TK-AD-08   | Gas Oil         | 28.0                     | 16                            |  
| Ahmadi Depot | TK-AD-09   | Gas Oil         | 28.0                     | 16                            |  
| Ahmadi Depot | TK-AD-10   | Premium         | 28.0                     | 16                            |  
| Ahmadi Depot | TK-AD-11   | Premium         | 28.0                     | 16                            |  
| Ahmadi Depot | TK-AD-12   | Super Premium   | 28.0                     | 16                            |  
| Ahmadi Depot | TK-AD-13   | Premium         | 28.0                     | 16                            |  
| Ahmadi Depot | TK-AD-14   | Super Premium   | 11.0                     | 11                            |  
| Ahmadi Depot | TK-AD-15   | Super Premium   | 11.0                     | 11                            |  
| Ahmadi Depot | TK-AD-16   | Super Premium   | 11.0                     | 11                            |  
| Ahmadi Depot | TK-AD-17   | Premium         | 11.0                     | 11                            |  
| Ahmadi Depot | TK-AD-18   | Premium         | 11.0                     | 11                            |  
| Ahmadi Depot | TK-AD-19   | Water           | 7.5                      | 10                            |  
| Ahmadi Depot | TK-AD-20   | Water           | 7.5                      | 10                            |  
| Ahmadi Depot | TK-AD-21   | Water           | 7.5                      | 10                            |  
| Ahmadi Depot | TK-AD-22   | Water           | 7.5                      | 10                            |  

## **3. Constraints**  

1. **Global Maintenance Limit**  
   - Maximum **3 tanks** can be under maintenance simultaneously.  

2. **Product-wise Availability**  
   - **Premium, Super Premium, Gas Oil, Water**: At least **70% of tanks per product** must remain operational.  
   - **Kerosene**: At least **50% of tanks** must remain operational.  
   - **UL 98**: Only one tank may be taken offline at a time (since there are only 2 tanks total).  

3. **Emergency Redundancy**  
   - **At least one tank per product must always remain in service**, regardless of stock percentage.  

4. **Maintenance Duration**  
   - Each tank must be unavailable for its full maintenance duration (see table).  

## **4. Optimization Goals**  

- Minimize **total maintenance time** while ensuring constraints are met.  
- Balance **resource allocation** to avoid bottlenecks.  

## **5. Assumptions**  

- Maintenance schedules are **non-preemptive** (once started, must complete without interruption).  
- No new tanks are added/removed during the planning horizon.  

## **6. Deliverables**  

1. **Maintenance Schedule**: A timeline indicating when each tank is taken offline.  
2. **Feasibility Check**: Verification that all constraints are satisfied.  
3. **Visualization**: Gantt chart or similar representation of maintenance periods.  
