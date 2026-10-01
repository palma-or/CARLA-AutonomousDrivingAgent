# Autonomous Vehicle Driving Project
Enhancing Navigation and Decision-Making Capabilities via Dedicated Behavioral Managers in CARLA

Targeted improvements to the CARLA autonomous agent: dedicated managers for pedestrians, static obstacles, complex junctions, traffic lights, and dynamic vehicle/cyclist interactions.

- [📚 Overview](#-overview)
- [🚀 Improvements Introduced](#-improvements-introduced)
- [⚙️ Control Systems](#️-control-systems)
- [📁 Project Structure](#-project-structure)
- [🛠️ Prerequisites](#-prerequisites)
- [🏃 How to Run](#-how-to-run)

## 📚 Project Overview
This project focuses on developing and enhancing an autonomous driving agent within the CARLA simulation environment. The initial baseline agent was highly limited, capable only of following predefined paths and stopping at immediately visible obstacles, which frequently led to collisions and failures in complex situations. 

To overcome these limitations, we designed a modular architecture consisting of specialized "Managers" that rely exclusively on CARLA's environmental data. These managers safely handle dynamic elements such as bicycles, parked vehicles, traffic lights, and road cones. 

Rigorous testing demonstrated massive improvements over the baseline:
- **Route 1 (Nighttime & Rain):** The enhanced agent completed 100% of the route at 40 km/h, achieving a near-perfect composed score of 99.02 with zero collisions.
- **Route 4 (Daytime):** The agent achieved a 97.28 composed score and 100% completion at an optimal speed of 30 km/h, safely navigating all pedestrian and layout hazards.

For detailed methodology, controller mathematics, and full results, please refer to the `Report.pdf` document.

## 🚀 Improvements Introduced (Agent Architecture)

### 🚶 Pedestrian Manager
Refined pedestrian detection and collision avoidance:
- Monitors a 12-meter detection radius near intersections and expands to 20 meters in standard driving contexts.
- Triggers automated emergency braking when pedestrians cross predefined critical safety thresholds.
- Utilizes a widened front field of view to detect pedestrians earlier, significantly improving reaction times on curves.

### 🚧 Static Obstacle & Overtake Manager
Enhanced handling of fixed hazards like construction zones and traffic cones via `overtake_manager.py`:
- Calculates dynamic overtaking trajectories for fully blocked lanes by estimating the required distance and predicting oncoming traffic using uniform acceleration equations.
- Cancels overtaking maneuvers immediately if a potential collision with oncoming traffic is detected.
- Applies controlled lateral offsets (+0.2 or -2) to safely bypass partial blockages without leaving the ego-lane.

### 🚦 Junction Manager
Sophisticated intersection navigation ensuring right-of-way compliance via `junction_manager.py`:
- Classifies intersections into four distinct structural types (e.g., straight/left, all directions) to adapt merging strategies.
- Monitors the active turn indicators of surrounding vehicles to anticipate their trajectories and minimize collision risks.

### ⛔ Stop Sign & Traffic Light Manager
Explicit traffic control handling:
- Detects traffic lights from 50 meters away, enabling smooth deceleration for red lights or maintained speed for green lights.
- Detects stop signs at 20 meters and performs a complete, mandatory halt at 2 meters.
- Analyzes stationary vehicles ahead to determine if they are waiting at a sign/light, explicitly preventing illegal and risky overtakes.

### 🚲 Vehicle & Bicycle Manager
Improved detection and following logic for dynamic actors:
- Implements Adaptive Cruise Control to maintain safe following distances in steady traffic.
- Detects bicycles and executes safe passing maneuvers maintaining a 2-meter lateral offset.
- Restricts all overtaking maneuvers on curves by analyzing the spatial coordinates and angular changes of upcoming waypoints.

## ⚙️ Control Systems
The agent leverages fine-tuned baseline controllers to guarantee smooth longitudinal and lateral movement:
- **Longitudinal Control:** PID Controller ($K_P = 0.888$, $K_I = 0.0768$, $K_D = 0.05$).
- **Lateral Control:** Stanley Controller ($K_V = 4.0$, $K_S = 1.0$).

## 📁 Project Structure
The repository is organized as follows:

```text
├── 📁 BehaviorManager/               # Specialized behavioral logic
│   ├── junction_manager.py           # Logic for handling intersections
│   └── overtake_manager.py           # Logic for overtaking and obstacle avoidance
├── 📁 carla_behavior_agent/          # Core agent components
│   ├── basic_agent.py
│   ├── basic_autonomous_agent.py
│   ├── behavior_agent.py
│   ├── behavior_types.py
│   ├── config_agent_basic.json
│   ├── controller.py                 # PID and Stanley controllers
│   ├── global_route_planner.py
│   ├── local_planner.py
│   ├── misc.py
│   └── utils.py
├── 📁 Final Results/                 # Final simulation evaluation results
│   ├── Route1_Final_Result.json
│   └── Route4_Final_Result.json
├── 📁 results/                       # Standard simulation outputs
│   └── simulation_results.json
├── 📁 Simulations/                   # Detailed logs and intermediate test results
│   ├── simulation_results_1_*.json
│   └── simulation_results_4_*.json
├── route_1_avddiem_*.xml             # Scenario 1 route definition
├── route_4_avddiem_*.xml             # Scenario 4 route definition
├── run_scenario.sh                   # Script to run a specific scenario
├── run_test.sh                       # Script to execute tests
├── server_http.py                    # HTTP server utility
├── speed.txt
├── Projects_Tasks.pdf
├── Report.pdf                        # Detailed project report
└── README.md
```

## 🛠️ Prerequisites
To correctly run this project and replicate the development environment, the following system and software requirements must be met:

📌 **Required Software**
- CARLA Simulator (Minimum version: 0.9.13)
- Docker (Recommended version: 20.10+)
- Python (Recommended version: Python 3.8 or higher)

📌 **Required Python Packages**
- `numpy`
- `matplotlib`
- `shapely`
- `carla`

## 🏃 How to Run

```bash
# Clone the repository
git clone [https://github.com/YourUsername/CARLA-Autonomous-Driving-Agent.git](https://github.com/YourUsername/CARLA-Autonomous-Driving-Agent.git)
cd CARLA-Autonomous-Driving-Agent

# Start the CARLA server (ensure your environment is properly set up)
# ...

# Launch the tests or specific scenarios using the provided scripts
./run_test.sh

# Or run a specific scenario
./run_scenario.sh
```

## 👨‍💻 Authors
- Giovanni Lamb 
- Palma Orlando
- Fabrizio Saturnino 
- Egidio Zottarelli 
