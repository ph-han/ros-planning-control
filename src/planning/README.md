# Planning
> polynomial-based planning  
> [optimal trajectories in a frenet frame (polynomial)](https://blog.phan.kr/posts/Optimal-Trajectories-in-a-Frenet-Frame/)

## Overview
- Local path planning using optimal trajectories in a frenet frame (polynomial)
- Publishing rate: 200 ms (velocity in m/s)

## Data Flow
- **Input (Subscribed topics):** `/global_path`, `/odom`, `/Ego_topic`, …
- **Processing steps:**
  - Preprocess the reference trajectory
  - Generate trajectories in the Frenet frame
  - Select the optimal valid trajectory
- **Output (Published topics):** `/opt_path`, `/valid_local_path`, `/plan_velocity_info`

## Structure
- `scripts/`
  - `global_path.py` – Global path publishing node
  - `local_path.py` – Local path publishing node

- `src/planning_pkg/`
  - `planner.py` – Generates trajectories
  - `config.py` – Parameters (planning time, weights, max [speed, accel, kappa], etc.)
  - `frenet.py` – Frenet ↔ World coordinate transformation
  - `polynomial.py` – Polynomial generation (quintic, quartic)
  - `frenet_path.py` – Definition of the FrenetPath class

- `msg/PlanVelocityInfo.msg` – 제어기 연동 메시지 (current speed, target speed)

## Configuration
- Refer to comments in the parameter description file (`planning/src/planning_pkg/config.py`).
- Most parameters used in planning (e.g., speed/acceleration limits, cost weights, etc.) can be configured and tuned here.

## Running
1. `roslaunch control control.launch`
2. `roslaunch planning_pkg planning.launch`


