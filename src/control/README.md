# Control

## Overview
- **Longitudinal control:** PID controller  
- **Lateral control:** Pure Pursuit  
- Publishes every 20 ms

## Data Flow
- **Input (Subscribed topics):** `/opt_path`, `/plan_velocity_info`, `/odom`
- **Processing steps:**
  - Calculate steering using Pure Pursuit with the optimal local path
  - Calculate acceleration and braking using PID control
- **Output (Published topic):** `/ctrl_cmd` (Accel, Brake, Steering)  
  → This topic is used as the control message in the MORAI simulator


## Structure
- `control/scripts/ego_control_pub.py` – Entry point for the control node; handles ROS communication and the main control loop
- `control/scripts/odom.py` – Publishes the `/odom` topic and performs dead reckoning when entering GPS shadow areas, considering side-slip effects
- `control/src/control/pure_pursuit.py` – Lateral control utility (Pure Pursuit)
- `control/src/control/pid.py` – Longitudinal PID controller
- `control/launch/control.launch` – Launch file for the control node (includes odom publisher)

## Configuration
- **Control parameters:**
  - **Pure Pursuit:** parameters are passed to the constructor  
    - `PurePursuit(wheel_base, max_steer, K_LD, min_ld)`  
    - After setting the minimum lookahead distance (`min_ld`), the lookahead distance is scaled proportionally to the current speed (`K_LD * curr_kph`)
  - **PID Controller:** parameters are passed to the constructor (based on m/s)  
    - `PIDController(p_gain, i_gain, d_gain)`  
    - Example: `self.pid = PIDController(1.08, 0.252, 1.08)`  
    - A maximum acceleration constant is defined inside the class (the output is limited to the range [0, 1])

## Running
- Ensure that the `/odom` and `/opt_path` topics are active  
- Run the control node:  
  ```bash
  roscd control && cd scripts
  python3 ego_control_pub.py
  ```

