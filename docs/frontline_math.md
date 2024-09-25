
## Principles behind time & cost saving

### Project duration reduction

For a **single task-resource pair**:

$$
\\text{duration (work-hours)} \\equiv { \\text{total units of work} \\over \\text{units per hour} }
$$

- If a task requires 5 men working for 10 days, that is, it requires 50 \`man-days\` of work.
- Then we assume it can be done by 10 men in 5 days.

- That is, we can adjust the duration of a task by adjusting the amount of resource assigned to it.

> (As this relationship may not always hold, you can declare any exceptions in our \`constraints input\`)

- Now, the description above may seem easy, that is because the complexity actually arises from [Combinatorics](https://en.wikipedia.org/wiki/Combinatorial_optimization):
    - When you have 10Ks of tasks, 100s of resources...
    - When you want to shorten the entire project's duration (which is determined by the [critical path](https://en.wikipedia.org/wiki/Critical_path_method) of tasks instead of a single task)...
    - When you want to reduce peaks of all resource distributions at the same time...
    - When you want to reduce all resource costs, each with different price and distribution...
    - When you also have many different types of constraints...
    - ...
- When you want to **do all of the above simultaneously**, what duration should each task have? how much of each type of resource should you assign to it?
    - It is simply mathematically impossible for any human to come up the optimal numbers from their head...

- Our AI's ability lies in **considering all of the tasks, resources, calendars, all their relationships, all your constraints, all your optimization targets at the same time to find out optimal solutions from infinite number of possibilities**

### Project cost reduction

> There isn't much we can do trying to reduce the cost of \`Material\` resources. If it takes 100 bricks to build a wall, we cannot ask you to take a few out just to save money. That would be unsafe.

- The cost reduction mainly comes from **making full use of** \`Labor\` and \`Equipment\` resources.

For a single \`Labor\` or \`Equipment\` resource:

$$
\\text{cost} \\equiv \\text{peak of distribution (units) in one period} \\times \\text{cost per unit in this period} 
$$

![](/assets/imgs/cost.png)

- If a project takes 1 month    
    - If the peak resource usage is 4 men or 4 machines, then you need to hire 4 men or rent 4 machines for 1 month, suppose each resource requires \\$10k per month, then you need to spend \\$40k on salaries or rental costs
    - However, if you can somehow reduce the peak value (while maintaining the same duration or even shortening the duration), you can save money because you need to hire fewer people / rent fewer machines.
        - (For example, one can reduce the peak value by shifting tasks around or lengthening their durations)
    - By reducing the resource distribution peak (leveling the resource), you are reducing the **idle time** (the period of time they are hired / rented, yet are not assigned to any tasks)

- Things get more complex when you have multiple resources, for example, due to various task-resource relationships, two resources might become \`entangled\` with each other, that is, making one distribution more efficient might cause the other to be less efficient, in this case we need to consider what are the prices for each of them, find tradeoffs and balances

- Our AI is perfect at handling such complexities.

> - We do not consider the case where a person or machine is paid hourly: you pay them if you need them for this hour, and let them go home and not pay them when you don't need them in the next hour. 
> - As we consider resource distributions at hour-level precision, resources should at least be paid daily for cost reduction to work. We provide the option to define **pay period** for each resource in our \`constraints input\`

### Conclusion

- In summary, we have explained the principles behind duration and cost reduction.
- As a real life example, the following shows the designs our AI has found in the solution space. We can see that it is possible to find solutions with shorter duration and lower cost at the same time via optimal resource allocation:

<img class="img1" src="/assets/imgs/optimizer_1b.png" />

### Classical optimization algorithms vs Frontline

- The problems we consider has a (simpler) textbook form known as [the Job-shop scheduling problem](https://en.wikipedia.org/wiki/Job-shop_scheduling), which is categorized as [NP-hard](https://en.wikipedia.org/wiki/NP-hardness) in Computer Science.

- At the very beginning of Frontline, we attempted using several standard optimization algorithms. However, we found that none of them could handle (or took forever to optimize) real-world projects with thousands of tasks. (due to [Curse of dimensionality](https://en.wikipedia.org/wiki/Curse_of_dimensionality))

- That is what drove us to develop our unique AI system which uses latest AI technologies to solve this problem in a completely different way.
    - We welcome any challenges to compete with us in terms of optimization speed and the optimality of solutions found.
