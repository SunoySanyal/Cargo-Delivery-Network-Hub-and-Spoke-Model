# Cargo Delivery Network Hub-and-Spoke Model

## Description

A concise overview: This repository presents optimization models and solutions for designing a cargo delivery network in Turkey using hub-and-spoke structures. It includes both single-hub and capacitated multi-hub formulations, implemented and solved in Python (Jupyter notebooks) with Gurobi.

This repository contains the formulation and solution for a cargo delivery network in Turkey using a hub-and-spoke structure. The data file `TurkishData.xlsx` provides:

- **Expected demand** between city pairs
- **Transportation cost** and **time** matrices
- **Setup costs** for each candidate hub location

All code is implemented in Python (Jupyter notebooks) and Gurobi for MILP.

---

## Part (a): Single-Hub Model

### Sets

- `I`: Set of all cities (nodes).

### Parameters

- `h_j`: Setup cost to open a hub at city *j*.
- `f_{ij}`: Unit transportation cost per unit flow from *i* to *j*.
- `w_{ij}`: Expected flow (demand) from *i* to *j*.

### Decision Variables

- `y_j ∈ {0,1}`: 1 if a hub is opened at city *j*.
- `x_{ij} ∈ {0,1}`: 1 if all flow from *i* is routed through hub *j*.

### Objective

Minimize total setup + transport cost:

![Objective](https://latex.codecogs.com/svg.image?%5Cmin%20%5Csum_%7Bi%5Cin%20I%7D%5Csum_%7Bj%5Cin%20I%7D%20f_%7Bij%7D%20w_%7Bij%7D%20x_%7Bij%7D%20%2B%20%5Csum_%7Bj%5Cin%20I%7D%20h_j%20y_j)

### Constraints

1. **One hub**:

   ![One hub](https://latex.codecogs.com/svg.image?%5Csum_%7Bj%5Cin%20I%7D%20y_j%20%3D%201)

2. **Assign flow only to open hub**:

   ![Assign flow](https://latex.codecogs.com/svg.image?x_%7Bij%7D%20%5Cle%20y_j%2C%20%5Cquad%20%5Cforall%20i%2Cj%5Cin%20I)

3. **All flow assigned**:

   ![All flow](https://latex.codecogs.com/svg.image?%5Csum_%7Bj%5Cin%20I%7D%20x_%7Bij%7D%20%3D%201%2C%20%5Cquad%20%5Cforall%20i%5Cin%20I)

4. **Binary**:  
   - `x_{ij} ∈ {0,1}`  
   - `y_j ∈ {0,1}`

---

## Part (b): Multi-Hub Model with Budget & Discount

### Additional Parameters

- `B = 2400`: Total budget for hub setup costs.
- `α = 0.5`: Inter-hub transfer discount factor.

### Decision Variables

- `y_k ∈ {0,1}`: 1 if a hub is opened at city *k*.
- `x_{ij,kl} ≥ 0`: Fraction of flow from *i* to *j* routed via hub pair *(k,l)*.

### Objective

Minimize total transport + setup cost:

![Objective b](https://latex.codecogs.com/svg.image?%5Cmin%20%5Csum_%7Bi%2Cj%5Cin%20I%2C%20i%5Cneq%20j%7D%5Csum_%7Bk%2Cl%5Cin%20I%7D%20%28d_%7Bik%7D%20+%20%5Calpha%20d_%7Bkl%7D%20+%20d_%7Blj%7D%29%20w_%7Bij%7D%20x_%7Bij%2Ckl%7D%20+%20%5Csum_%7Bk%5Cin%20I%7D%20h_k%20y_k)

### Constraints

1. **Budget**:

   ![Budget](https://latex.codecogs.com/svg.image?%5Csum_%7Bk%5Cin%20I%7D%20h_k%20y_k%20%5Cle%20B)

2. **Flow conservation**:

   ![Flow conservation](https://latex.codecogs.com/svg.image?%5Csum_%7Bk%2Cl%5Cin%20I%7D%20x_%7Bij%2Ckl%7D%20%3D%201%2C%20%5Cquad%20%5Cforall%20i%5Cneq%20j)

3. **Route only via open hubs**:

   ![Route open](https://latex.codecogs.com/svg.image?x_%7Bij%2Ckl%7D%20%5Cle%20y_k%2C%20x_%7Bij%2Ckl%7D%20%5Cle%20y_l)

4. **Nonnegativity & binary**:  
   - `x_{ij,kl} ≥ 0`  
   - `y_k ∈ {0,1}`

---

## Files & Usage

- **Data**: `TurkishData.xlsx`  
- **Code**:  
  - `q2a_single_hub.ipynb` — Single-hub algorithm (Problem 2a)  
  - `q2b_multi_hub.ipynb` — MILP model with Gurobi (Problem 2b)

### Reproduce Results

1. Clone this repository.  
2. Place `TurkishData.xlsx` in the project root.  
3. Install dependencies:  
   ```bash
   pip install -r requirements.txt
