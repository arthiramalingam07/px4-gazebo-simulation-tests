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
