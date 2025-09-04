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

First, the whole environment is divided into small blocks. While several essential parameters should be measured within each block, such as temperature, humidity, and air velocity, the importance of measuring these parameters can vary across blocks. Factors such as the presence and location of residents, as well as the number of individuals in each block, influence the significance of each measurement [5][7][59]. Additionally, monitoring certain areas can impact energy efficiency and help identify undesirable behaviors, such as residents leaving doors or windows open, that may compromise environmental control [11][84]. Therefore, measuring specific parameters in some blocks may be of greater importance than in others. Moreover, the relative importance of temperature, humidity, and air velocity can differ even within the same block. Accordingly, one of the key inputs to the proposed method is the importance weight associated with measuring each parameter in each block. This is represented by w_pt, which denotes the weight indicating the importance of measuring parameter t in block p.
When a sensor is installed in a block, it can directly measure parameters such as temperature, humidity, and air velocity in that specific block. In addition, it can provide virtual measurements for adjacent blocks. The accuracy of these measurements depends on several factors, including the type of sensor used, the characteristics of the environment, and the method employed for virtual estimation. Another key input to the proposed method is the measurement accuracy for each parameter in each block, assuming a sensor is installed nearby. This parameter must be calculated before the optimization process begins. This is denoted by λ_klpt, which represents the accuracy of measuring parameter of parameter t in block p when a sensor of type k is installed at location l.
Based on this input data, the significance of each block, the importance of each parameter, and the measurement accuracy, the proposed method determines the optimal sensor layout, including the number, type, and placement of sensors.
