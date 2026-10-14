# PX4 Autopilot & Gazebo Harmonic Simulation Work

This repository showcases two automated software-in-the-loop (SITL) simulation testcases developed using **PX4 Autopilot** and **Gazebo Harmonic**.

---

## 🛠️ Project 1: Aerodynamic Wind Simulation
* **Objective:** Implement dynamic, localized wind vectors within Gazebo Harmonic and bridge the telemetry directly into the PX4 Flight Review log architecture.
* **C++ Implementation:** Because standard PX4 firmware does not route global wind packets out of Gazebo Harmonic natively, I modified the flight controller's internal C++ translation layers (`GZBridge.cpp` and `GZBridge.hpp`) to establish a custom uORB network subscription mapping.
* **Key Achievements:**
  * Created a customized `windy.sdf` world setting linear wind vectors (`-2 0 0`) and physical `atmospheric_density`.
  * Wrote C++ callback hooks to parse `gz::msgs::Wind` packets into PX4's structural metrics (`windspeed_north`, `windspeed_east`).
  * Validated vehicle attitude control changes where the quadcopter pitched/rolled to physically battle aerodynamic drag forces.

---

## 🔋 Project 2: Battery Check & Failsafe Recovery Validation
* **Objective:** Verify autonomous system safety responses under critical energy depletion profiles.
* **Configurations:** Utilized the flight controller's internal parameter arrays to control simulated energy behaviors dynamically.
* **Testing Routine:**
  * Enabled simulated capacity depletion via `SIM_BAT_DRAIN`.
  * Configured battery failsafe action profiles (`COM_LOW_BAT_ACT`) to trigger an immediate Return-to-Land (RTL).
  * Validated flight controller compliance when capacity dropped to the 5% threshold, resulting in a safe autonomous return to the primary launch coordinate.

---

## 📁 Repository Structure
* `windy.sdf`: Customized Gazebo simulation environment file.
* `modified_files/gz_bridge/`: Core C++ architecture upgrades routing wind telemetry into the uORB framework.


Commands:
x     SIM_BAT1_DRAIN [905,1510] : -1.0000
x     SIM_BAT1_MIN_PCT [906,1511] : -1.0000
x     SIM_BAT2_DRAIN [907,1512] : -1.0000
x     SIM_BAT2_MIN_PCT [908,1513] : -1.0000
x     SIM_BAT3_DRAIN [909,1514] : -1.0000
x     SIM_BAT3_MIN_PCT [910,1515] : -1.0000
x     SIM_BAT_DRAIN [911,1516] : 60.0000
x +   SIM_BAT_MIN_PCT [912,1517] : 5.0000

param show SIM_BAT_*
param set SIM_BAT_DRAIN 60.0

Path to local -\\wsl.localhost\Ubuntu\home\rthi\PX4-Autopilot\Tools\simulation\gz
Path to Logs -\\wsl.localhost\Ubuntu\home\rthi\PX4-Autopilot\build\px4_sitl_default\rootfs\fs\log

wind file :nano ~/PX4-Autopilot/Tools/simulation/gz/worlds/windy.sdf

To launch PX4 ,cd PX4-Autopilot
make px4_sitl gz_x500

To view UI
gz sim -g

cd ~/PX4-Autopilot && PX4_GZ_WORLD=windy make px4_sitl gz_x500

Delete Logs
cd ~/PX4-Autopilot
rm -f logs/*.ulg
find ~ -type f -name "*.ulg" -delete

List of log files 
find ~ -type f -name "*.ulg"

Wind world file : nano Tools/simulation/gz/worlds/windy.sdf
Default world file : 

copy text from nano : grep -A 12 -B 2 '<wind>' ~/PX4-Autopilot/Tools/simulation/gz/worlds/windy.sdf
