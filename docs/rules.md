# Frontline Optimization Principle

## 1. Supported project file formats
The following project file formats are supported:
- `.xer` for Primavera P6 (version 19.12 and later)
- `.xml` for Microsoft Project

## 2. Reduction of duration
    To reduce the duration of a project, there are many factors that affect it, such as resources, technology, and environment. In actual projects, if you want to shorten the duration, you can take the following measures:

- Increase resources: \
    Increasing resources can effectively shorten the project duration because the more resources there are, the larger the workload of the project, so it is possible to reduce the duration by allocating more resources to the project.
    - Rules:
        - Base rule：
            $$
                duration = \frac{taskResourcesDemand}{resourcesPerHour}
            $$
            If we can increase the **resourcesPerHour** and maintain the *totalResourcesDemand*, the duration will be reduced.

        - Rule 1:\
            For the entire project, while keeping the total resource demand unchanged and not changing the logical relationship between tasks, we adjust the duration of tasks according to the impact of tasks on the total project duration. When the duration of a task needs to be shortened, the demand can be met by increasing the resource utilization rate.
            $$
            \begin{aligned}
                newStart &= \left\{ 
                                \begin{aligned}
                                    predecessorStart + Lag,&\ if\ logic = SS \\
                                    predecessorFinish + Lag,&\ if\ logic = FS \\
                                \end{aligned}
                            \right. \\

                newDuration &= durationRatio \times remainingDuration \\

                newFinish &= \left\{ 
                                \begin{aligned} 
                                    newStart + newDuration,&\ if\ logic = SS\ or\ FS \\
                                    predecessorFinish + Lag,&\ if\ logic = FF \\
                                    predecessorStart + Lag,&\ if\ logic = SF \\
                                \end{aligned}
                            \right. \\

                totalDuration &= Max(\text{newFinish of all tasks}) - Min(\text{newStart of all tasks}) \\
            \end{aligned}
            $$
            _note:_
            - _The **newStart** is the start time of the task after the adjustment._
            - _The **newDuration** is the duration of the task after the adjustment._
            - _The **newFinish** is the finish time of the task after the adjustment._
            - _The **totalDuration** is the total duration of the project after the adjustment._
            - _The **durationRatio** is the ratio of the newDuration to the remainingDuration._
            - _The **remainingDuration** is the duration of the task before the adjustment._

            The degree to which durationRatio affects totalDuration is given by the following formula:
            $$
            \begin{aligned}
                &\nabla = \frac{\partial totalDuration}{\partial durationRatio} = \frac{\partial totalDuration}{\partial newDuration} \times \frac{\partial newDuration}{\partial durationRatio} \\

                &\text{If } \nabla > 0, \text{ then the change of durationRatio is in the same direction as totalDuration.}\\
                &\text{If } \nabla < 0, \text{ then the change of durationRatio is in the opposite direction as totalDuration.}\\
                &\text{If } \nabla = 0, \text{ then the change of durationRatio has no effect on totalDuration.}
            \end{aligned}
            $$
    - Example:
        - Task A: 10 hours, 20 units, 2 units/hour
        - Task B: 20 hours, 50 unit, 2.5 units/hour
        - Task C: 15 hours, 45 units, 3 units/hour
        - Total resource demand: 115 units
        - Logic relationship: $A \stackrel{FS}{\longrightarrow} B$ $\stackrel{FS}{\longrightarrow} C$
        - Total duration: 45 hours
        - If we want to shorten the duration to 40 hours, we can increase the resource utilization rate of Task A to 2.5 units/hour, then the duration of Task A will be reduced to 8 hours, the durationRatio will be 0.8, and the resource utilization rate of Task C to 3.75 units/hour, then the duration of Task C will be reduced to 12 hours, the durationRatio will be 0.8, and the Task B remains unchanged, the durationRatio will be 1.\
        The total resource demand is still 115 units, and the total duration is 40 hours.

## 3. Reduction of cost

    To reduce the cost of a project, there are several factors that affect it, such as price, resources, and duration. In actual projects, if you want to reduce the cost, you can take the following measures:

- Reduce the idle time of non-material resources: \
    Reasonably arrange the use of non-material resources, avoid waste of resources, improve the utilization rate of resources, reduce the idle time of resources, and reduce the cost of the project.

    - Rules:
        - Base rule:
            $$
            	\begin{aligned}
                    totalCost &= materialCost\ +\ nonmaterialCost \\
                    &= \sum_{i=1}^{n_m} material\_cost_i + \sum_{j=1}^{m} nonmaterial\_cost_j \\
                \end{aligned}
            $$
            In the above formula, $material\_cost_i$ is the cost of the i-th material resource, and $nonmaterial\_cost_j$ is the cost of the j-th non-material resource. The cost of material resources is fixed and unchanged. If we can reduce the cost of non-material resources, the total cost will be reduced.

        - Rule 1:\
            For the entire project, while keeping the total resource demand unchanged and not changing the logical relationship between tasks, we adjust the duration of tasks according to the impact of tasks on the total project cost. When the cost of a task needs to be reduced, the demand can be met by increasing the utilization rate of non-material resources and reducing their idle time.
            
            Why can the cost of a task be reduced by increasing the utilization rate of non-material resources? Because the cost of non-material resources is generally calculated on a monthly or daily basis, rarely on an hourly basis. Therefore, assuming that the cost calculation period is monthly, when the actual working time of a non-material cost is 2 months (plus 0.5 months of idle time in the middle), it still needs to pay 3 months of cost. If a scheduling scheme is used to make the working time of the resource continuous for 2 months, only 2 months of cost is required.

            $$
            \begin{aligned}
                &\begin{aligned}
                    nonmaterial\_cost_j &= \sum_{k=1}^{n} cost_{k,j} \\
                    &= \sum_{k=1}^{n} \left( units_{k,j} \times costPerMonth_j \right) \\
                \end{aligned}\\

                &units_{k,j} = \max_{d} \left(resourceDemandDay_{d,k,j} \right)\\

                &resourceDemandDay_{d,k,j} = \max_{h} \left(resourceDemandHour_{h,d,k,j}\right)\\

                &resourceDemandHour_{h,d,k,j} = \sum_{i=1}^{m} \left( resourceDemandH_{i,h,d,k,j} \right)\\
                
                &resourceDemandH_{i,h,d,k,j} = \left\{ 
                                                    \begin{aligned} 
                                                        \frac{taskResourcesDemand_{i,j}}{newDuration_i}&,\ if\ newStart \leqslant h \leqslant newFinish \\
                                                        0&,\ otherwise \\
                                                    \end{aligned}
                 \right.\\

                 &newDuration_i = durationRatio_i \times remainingDuration_i \\
            \end{aligned}
            $$

            _note:_
            - 
                 i - task_i, j - resource_j, h - hour_h, d - day_d, k - month_k
            - _The **$nonmaterial\_cost_j$** is the cost of the j-th non-material resource._
            - _The **$cost_{k,j}$** is the cost of the j-th non-material resource in the k-th month._
            - _The **$units_{k,j}$** is the total units of the j-th non-material resource in the k-th month._
            - _The **$resourceDemandDay_{d,k,j}$** is the total resource demand of the j-th non-material resource in the k-th month on the d-th day._
            - _The **$resourceDemandHour_{h,d,k,j}$** is the total resource demand of the j-th non-material resource in the k-th month on the d-th day and h-th hour._
            - _The **$resourceDemandH_{i,h,d,k,j}$** is the resource demand of the i-th task for the j-th non-material resource in the k-th month on the d-th day and h-th hour._
            - _The **$newDuration_i$** is the duration of the i-th task after the adjustment._
            - _The **$durationRatio_i$** is the ratio of the newDuration to the remainingDuration of the i-th task._

            The degree to which durationRatio affects totalCost is given by the following formula:
            $$
            \begin{aligned}
                &\begin{aligned}
                    \nabla = \frac{\partial totalCost}{\partial durationRatio} = &\frac{\partial totalCost}{\partial nonmaterial\_cost} \times \frac{\partial nonmaterial\_cost}{\partial units} \times \\ &\frac{\partial units}{\partial resourceDemandDay} \times \frac{\partial resourceDemandDay}{\partial resourceDemandHour} \times \\ &\frac{\partial resourceDemandHour}{\partial resourceDemandH} \times \frac{\partial resourceDemandH}{\partial newDuration} \times \\ &\frac{\partial newDuration}{\partial durationRatio} \\
                \end{aligned}\\

                &\text{If } \nabla > 0, \text{ then the change of durationRatio is in the same direction as totalCost.}\\

                &\text{If } \nabla < 0, \text{ then the change of durationRatio is in the opposite direction as totalCost.}\\

                &\text{If } \nabla = 0, \text{ then the change of durationRatio has no effect on totalCost.}
            \end{aligned}
            $$

## 4. Resource distribution, reduction of duration and cost
    
        In actual projects, the distribution of resources is often related to the reduction of duration and cost. If you want to reduce both the duration and cost of a project, you can take the following measures:

- Adjust the distribution of resources: \
    By adjusting the distribution of resources, the idle time of resources can be reduced, the utilization rate of resources can be improved, the duration of the project can be shortened, and the cost of the project can be reduced.

    - Rules:
        - Base rule:
            $$
                nonmaterialCost = \left\lceil
                                    resourceWorkingTime_{month}
                                \right\rceil \times resourceUnitPrice_{month}
            $$
            In the above formula, $resourceWorkingTime_{month}$ is the working time of the resource (in months), and $resourceUnitPrice_{month}$ is the unit price of the resource (per month). If we can reduce the idle time of the resource and maintain the unit price of the resource, the cost of the resource will be reduced.
        
        - Rule 1: Same as 3. Reduction of cost, Rule 1.

        - Example:\
            Task A: from 2024-01-01 to 2024-01-10, resource: labor 1 units/hour\
            Task B: from 2024-01-11 to 2024-02-05, resource: labor 1 units/hour\
            Task C: from 2024-01-28 to 2024-02-04, resource: labor 1 units/hour\
            resource unit price: 1000$/month\
            Logic relationship: $A \stackrel{FS}{\longrightarrow} B$, $A \stackrel{FS+Lag:17d}{\longrightarrow} C$\
            Total resource demand: 2 people in January, 2 
            people in February\
            Total cost: 4000$\

            - If we want to shorten the duration to within 1 month and reduce the cost to 3000$, we can adjust the distribution of resources as follows:\
            Task A: from 2024-01-01 to 2024-01-04, resource: labor 3 units/hour\
            Task B: from 2024-01-04 to 2024-01-17, resource: labor 2 units/hour\
            Task C: from 2024-01-21 to 2024-01-25, resource: labor 2 units/hour\
            Total resource demand: 3 people in January\
            Total cost: 3000$


            - If we want to shorten the duration to 1 month and reduce the cost to 2000$, we can adjust the distribution of resources as follows:\
            Task A: from 2024-01-01 to 2024-01-05, resource: labor 2 units/hour\
            Task B: from 2024-01-06 to 2024-01-31, resource: labor 1 units/hour\
            Task C: from 2024-01-23 to 2024-01-30, resource: labor 1 units/hour\
            Total resource demand: 2 people in January\
            Total cost: 2000$

            - ...

            There are many similar resource distribution optimization schemes like this, Frontline finds these schemes through AI algorithms, thereby reducing the idle time of resources, improving the utilization rate of resources, shortening the duration of the project, and reducing the cost of the project.
