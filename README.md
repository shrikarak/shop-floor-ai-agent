# AI Agent for Autonomous Shop Floor Management

**Copyright (c) 2026 Shrikara Kaudambady. All rights reserved.**

## 1. Introduction

In a modern smart factory, operational efficiency is key. Downtime due to machine failure or stockouts can be incredibly costly. This project demonstrates a powerful AIOps (AI for Operations) solution: a **Shop Floor Manager AI Agent**.

This Jupyter Notebook simulates an autonomous agent that acts as a virtual manager for a factory production line. It continuously monitors key production metrics, diagnoses potential issues based on pre-defined thresholds, and takes corrective actions—such as re-ordering inventory or scheduling maintenance—to ensure the line runs smoothly and efficiently.

## 2. The Solution Explained: An Agent-Based Simulation

This solution is built as a dynamic, closed-loop system where the AI agent perceives, decides, and acts within a simulated environment.

### 2.1. The Simulated Environment
A `ShopFloorEnvironment` class models the state of the factory floor. This state is dynamic and changes with every production cycle. The key state variables are:
*   **Inventory:** A dictionary tracking the stock levels of raw materials (e.g., `screws`, `casings`).
*   **Machine Health:** A `wear` attribute that increases with each cycle, representing the degradation of a machine over time.
*   **Production Yield:** The percentage of high-quality units produced, which is affected by factors like machine wear.

### 2.2. The Agent's Toolkit
The agent interacts with its environment using a set of defined "tools," which in the real world would be API calls to various factory systems.
*   `get_all_metrics()`: The agent's "eyes"—it uses this to perceive the current state of the environment.
*   `order_supplies()`: Simulates an API call to an ERP system to replenish low inventory. The simulation includes a lead time for the delivery.
*   `dispatch_maintenance()`: Simulates an API call to a CMMS (Computerized Maintenance Management System) to schedule a repair, which resets machine `wear` to zero but takes the machine offline for one cycle.
*   `adjust_machine_params()`: A tool to perform a minor, real-time tweak to temporarily boost yield at the cost of slightly increased machine wear.

### 2.3. The Agent's Decision-Making
The `ShopFloorManagerAgent` contains the core "brain" of the operation. In each cycle, it runs through a prioritized, rule-based decision tree:
1.  **Is machine health critical?** If `wear` exceeds a high threshold (e.g., 85%), the agent's highest priority is to schedule maintenance to prevent a catastrophic failure.
2.  **Is inventory low?** If not, the agent then checks if any raw material stock is below a safety threshold. If so, it places a re-order. It also tracks pending orders to avoid redundant requests.
3.  **Is production yield suffering?** If not, the agent checks if the quality yield has dropped below its target. If so, it attempts a minor parameter adjustment as a temporary fix.
4.  **If all is well,** the agent logs that all metrics are normal and takes no action.

The notebook runs this simulation for 100 cycles and plots the results, clearly visualizing how the agent's interventions affect the state of the factory over time.

## 3. How to Use the Notebook

### 3.1. Prerequisites

This project uses standard Python data science libraries. You will need:

```bash
pip install numpy pandas matplotlib seaborn
```

### 3.2. Running the Notebook

1.  Clone this repository:
    ```bash
    git clone https://github.com/shrikarak/shop-floor-ai-agent.git
    cd shop-floor-ai-agent
    ```
2.  Start the Jupyter server:
    ```bash
    jupyter notebook
    ```
3.  Open `shop_floor_simulation.ipynb` and run all cells. The notebook will print a log of the agent's diagnosis and actions for each cycle, then render graphs showing the full simulation history.

## 4. Real-World Deployment

This simulation is a blueprint for a real-world AIOps monitoring and automation system.

1.  **Data Integration:** The agent's `get_all_metrics` tool would be connected to live data streams instead of a simulation.
    *   Inventory data would be pulled from an ERP or Warehouse Management System (WMS) database.
    *   Machine health data would come from IoT sensors on the equipment (e.g., vibration, temperature, pressure sensors).
    *   Yield data would come from the production line's quality control systems.

2.  **Tool Integration:** The action tools would be implemented to trigger real-world processes.
    *   `order_supplies` would create a purchase order via an API call to the company's ERP system (e.g., SAP, Oracle).
    *   `dispatch_maintenance` would create a work order in a CMMS like IBM Maximo or SAP PM.

3.  **Advanced Decision-Making:** The simple `if/then` rules in this simulation could be upgraded to a more sophisticated AI model. For example, a **Reinforcement Learning (RL)** agent could be trained to learn the optimal policy over time, making more nuanced trade-offs between tweaking parameters, scheduling maintenance, and ordering supplies to maximize the long-term average yield.
