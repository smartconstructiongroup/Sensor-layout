# Sensor-layout
The layout and functionality of air quality measurement sensors play a significant role in building control systems and occupant comfort. While prior research has predominantly concentrated on the placement and density of sensors, limited emphasis has been placed on evaluating their accuracy, multifunctionality, and cost-efficiency. To address these factors, this paper proposes an Integer Linear Programming (ILP) bi-model that determines the optimal sensor layout for coverage and cost-efficiency. The ILP model achieves a balance between cost and coverage using a Leader-Follower Approach. This approach enables attaining the required coverage at the lowest possible cost or maximizing coverage within a fixed budget. The proposed bi-model accounts for sensor measurement accuracy at various locations to meet energy efficiency and occupant satisfaction requirements. By combining the ILP bi-model with the Leader-Follower approach, this study reduces computational complexity and runtime without compromising solution accuracy in determining the sensor layout. The results demonstrate the model’s ability to rapidly identify an optimal sensor configuration that enables coverage assurance and cost efficiency.

<img width="1043" height="620" alt="image" src="https://github.com/user-attachments/assets/8fd2bdf1-78df-466e-a241-b162586fabe1" />


# Proposed Method Code and Usage Instructions

The proposed method has been implemented in **IBM® ILOG CPLEX 12.10**. Four files are shared in this repository: two files contain the ILP models, and two files contain the corresponding data files.

### ILP Models

1. **Cost Optimization Model**

   * This ILP model optimizes the cost of sensor placement.
   * The coverage level is considered as an input, and the model provides the **most cost-effective sensor layout**.
   * The required input data for this model is provided in the file `Cost model data`.

2. **Coverage Optimization Model**

   * This ILP model optimizes the coverage of the environment.
   * The budget constraint is considered as an input, and the model provides a layout that achieves the **maximum coverage within the given budget**.
   * The required input data for this model is provided in the file `Coverage model data`.

### Usage Strategies

There are two strategies for using these models:

1. **When a specific coverage level is required:**

   * First, solve the **Cost Optimization Model** to obtain the optimal cost for the desired coverage level.
   * Use this cost as the budget constraint for the **Coverage Optimization Model**.
   * Solving the **Coverage Optimization Model** allows increasing the coverage level **without increasing the budget**.

2. **When a budget constraint exists:**

   * First, solve the **Coverage Optimization Model** to obtain the maximum coverage achievable within the given budget.
   * Use this optimal coverage as a constraint in the **Cost Optimization Model**.
   * Solving the **Cost Optimization Model** allows reducing the costs **without decreasing the coverage level**.

### Notes on Input Parameters

First, the whole environment is divided into small blocks. While several essential parameters should be measured within each block, such as temperature, humidity, and air velocity, the importance of measuring these parameters can vary across blocks. Factors such as the presence and location of residents, as well as the number of individuals in each block, influence the significance of each measurement. Additionally, monitoring certain areas can impact energy efficiency and help identify undesirable behaviors, such as residents leaving doors or windows open, that may compromise environmental control. Therefore, measuring specific parameters in some blocks may be of greater importance than in others. Moreover, the relative importance of temperature, humidity, and air velocity can differ even within the same block. Accordingly, one of the key inputs to the proposed method is the importance weight associated with measuring each parameter in each block. This is represented by w_pt, which denotes the weight indicating the importance of measuring parameter t in block p.
When a sensor is installed in a block, it can directly measure parameters such as temperature, humidity, and air velocity in that specific block. In addition, it can provide virtual measurements for adjacent blocks. The accuracy of these measurements depends on several factors, including the type of sensor used, the characteristics of the environment, and the method employed for virtual estimation. Another key input to the proposed method is the measurement accuracy for each parameter in each block, assuming a sensor is installed nearby. This parameter must be calculated before the optimization process begins. This is denoted by λ_klpt, which represents the accuracy of measuring parameter of parameter t in block p when a sensor of type k is installed at location l.
Based on this input data, the significance of each block, the importance of each parameter, and the measurement accuracy, the proposed method determines the optimal sensor layout, including the number, type, and placement of sensors.



# Case Study and Results Summary

This repository includes a case study demonstrating the proposed **sensor placement optimization models**. The study was conducted on a single floor of a university campus in Sydney, Australia, including classrooms, computer labs, and lecture theaters.
 <img width="964" height="1070" alt="image" src="https://github.com/user-attachments/assets/00a1daa2-e699-47b1-9230-1e984ae880e2" />

**Fig. 2. Floor Layout of the Case Study Area.**


### Case Study Setup

* **Candidate Locations:** 29 potential sensor installation points (red circles in Fig. 2).
* **Blocks:** 55 environment blocks with similar temperature, humidity, and air velocity conditions (blue areas in Fig. 2).
* **Sensor Types:** Nine types of sensors (simple and multifunctional, contact and non-contact) measuring temperature, humidity, and air velocity. Sensor details, including accuracy and cost, are provided in Table 3.
* **Importance of Coverage:** Each block has a priority value (1–20) based on occupancy, usage, and energy contribution.

  **Table 2. The importance of covering the blocks in Fig. 2**

| Location | Temperature | Humidity | Air Velocity | Location | Temperature | Humidity | Air Velocity |
|----------|------------|---------|-------------|---------|------------|---------|-------------|
| 1        | 4          | 1       | 3           | 29      | 14         | 8       | 2           |
| 2        | 3          | 1       | 3           | 30      | 7          | 4       | 1           |
| 3        | 3          | 1       | 3           | 31      | 10         | 2       | 2           |
| 4        | 3          | 1       | 3           | 32      | 8          | 3       | 3           |
| 5        | 5          | 1       | 3           | 33      | 8          | 2       | 10          |
| 6        | 6          | 1       | 3           | 34      | 4          | 1       | 0           |
| 7        | 6          | 1       | 3           | 35      | 2          | 0       | 2           |
| 8        | 20         | 10      | 4           | 36      | 2          | 1       | 3           |
| 9        | 20         | 10      | 4           | 37      | 2          | 0       | 2           |
| 10       | 20         | 10      | 4           | 38      | 4          | 1       | 3           |
| 11       | 20         | 10      | 4           | 39      | 4          | 1       | 3           |
| 12       | 20         | 10      | 4           | 40      | 4          | 2       | 1           |
| 13       | 20         | 10      | 4           | 41      | 6          | 4       | 2           |
| 14       | 8          | 5       | 2           | 42      | 5          | 3       | 2           |
| 15       | 16         | 10      | 4           | 43      | 6          | 5       | 2           |
| 16       | 16         | 10      | 4           | 44      | 6          | 5       | 2           |
| 17       | 16         | 10      | 4           | 45      | 4          | 2       | 1           |
| 18       | 16         | 10      | 4           | 46      | 18         | 10      | 4           |
| 19       | 18         | 10      | 4           | 47      | 18         | 10      | 4           |
| 20       | 4          | 2       | 1           | 48      | 8          | 5       | 2           |
| 21       | 2          | 1       | 0           | 49      | 6          | 4       | 2           |
| 22       | 2          | 1       | 0           | 50      | 6          | 4       | 2           |
| 23       | 17         | 9       | 4           | 51      | 4          | 2       | 1           |
| 24       | 4          | 2       | 1           | 52      | 4          | 2       | 1           |
| 25       | 10         | 5       | 2           | 53      | 6          | 4       | 2           |
| 26       | 2          | 0       | 3           | 54      | 5          | 3       | 2           |
| 27       | 2          | 0       | 3           | 55      | 2          | 0       | 2           |
| 28       | 2          | 0       | 3           | -       | -          | -       | -           |


**Table 3. Details of candidate sensors for installation**
| Sensor | Type                    | Temperature | Humidity | Velocity | Cost ($) |
|--------|------------------------|------------|---------|---------|----------|
| 1      | Simple non-contact      | 98%        | -       | -       | 400      |
| 2      | Simple non-contact      | 97%        | -       | -       | 300      |
| 3      | Simple contact          | 98%        | -       | -       | 200      |
| 4      | Simple contact          | 97%        | -       | -       | 150      |
| 5      | Simple contact          | -          | 97%     | -       | 300      |
| 6      | Simple contact          | -          | -       | 95%     | 400      |
| 7      | Multifunctional contact | 98%        | 97%     | -       | 450      |
| 8      | Multifunctional contact | 98%        | -       | 95%     | 550      |
| 9      | Multifunctional contact | 98%        | 97%     | 95%     | 800      |


ر

### Implementation

* The **ILP models** were implemented in **IBM® ILOG CPLEX 12.10** via the OPL interface.
* Hardware: Windows® 10, 8 GB RAM, Core i5 CPU.
* **Leader-Follower Approach:** Two strategies implemented:

  1. **Minimum Coverage Assurance Strategy:** Input coverage = 60%.
  <img width="940" height="1107" alt="image" src="https://github.com/user-attachments/assets/1bd28e14-7884-4431-8435-cff5096433bc" />

**Fig. 3. Selected locations for sensor installation in the Minimum Coverage Assurance Strategy**

     * Minimum cost = \$2,550 (Cost Optimization Model).
     * Coverage can be increased to 61.3% at a reduced cost of \$2,250 (Coverage Optimization Model).
     * 11 sensors installed at selected locations.
  2. **Budget Constraint Strategy:** Input budget = \$4,000.
<img width="940" height="1105" alt="image" src="https://github.com/user-attachments/assets/2cf4f6e2-1a1e-4ecf-bbf6-09555eccc037" />

**Fig. 4. Selected locations for sensor installation in the Budget Constraint Strategy**

     * Maximum achievable coverage = 72.5% (Coverage Optimization Model).
     * 16 sensors installed at selected locations.

### Coverage–Cost Analysis

* Running the Cost Optimization Model 100 times produced a **coverage-cost curve** (Fig. 5).
<img width="1004" height="493" alt="image" src="https://github.com/user-attachments/assets/e6e3d612-a62b-40f9-896e-eedacf322779" />
**Fig. 5. Coverage–cost graph for case study**
* Insights:

  * Initial coverage improvements are highly cost-effective (up to \~40%).
  * Coverage beyond 80% requires sharply increasing costs, indicating diminishing returns.
  * Example: \$5,000 budget → 77% coverage; \$3,500 → 70% coverage.


