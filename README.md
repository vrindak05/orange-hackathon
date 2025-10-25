# orange-hackathon
# Network Congestion Optimization using HACO and QoS Adjustments

This project implements a simulation framework for **network congestion optimization** using intelligent routing and bandwidth management strategies. The system dynamically monitors router congestion levels and adjusts network paths and bandwidths to maintain optimal flow between servers and users.

---

## 🚀 Overview

The notebook `final_orange.ipynb` demonstrates three coordinated mechanisms:

1. **Dynamic Rerouting (HACO)**
   Implements a **Hybrid Ant Colony Optimization (HACO)** algorithm to reroute packets in real time.
   The routing cost depends on:

   * Edge capacities (static cost)
   * Node congestion penalties (dynamic cost)

   The algorithm continuously updates pheromones to prefer efficient, less congested routes.

2. **Bandwidth Adjustment**
   Adjusts link capacities dynamically:

   * Increases bandwidth between routers and underloaded neighbors.
   * Enhances QoS for congested links when alternative routes are not feasible.

3. **Policing (QoS Enforcement)**
   Applies policing policies when no alternate paths exist, ensuring fairness and controlled throughput.

---

## 🧠 Algorithm Summary

### Hybrid Ant Colony Optimization (HACO)

* Each “ant” probabilistically explores paths from `Server` to `User1–User4`.
* Transition probability is determined by pheromone intensity and inverse of edge cost.
* Over multiple iterations:

  * Good paths gain pheromone reinforcement.
  * Poor paths lose pheromone through evaporation.
* Edge costs include congestion penalties for highly loaded routers.

### Bandwidth & Policing Logic

* Periodically reads congestion data from `router_congestion_int.csv`.
* At each time step:

  * **High congestion (>70%)** → increase link capacity to underloaded neighbors (load <40%).
  * **No underloaded neighbors** → apply **policing** (QoS scaling).
  * Logs bandwidth adjustments and router statuses.

---

## 🧩 Dependencies

Ensure the following Python packages are installed:

```bash
pip install pandas networkx numpy
```

---

## 📂 File Structure

```
final_orange.ipynb          # Main notebook
router_congestion_int.csv   # Input congestion data (example)
README.md                   # Project documentation
```

---

## ⚙️ Usage

1. **Prepare congestion data:**
   The CSV should include columns:

   ```
   Time, RouterA, RouterB, RouterC
   ```

   Each row represents a time snapshot of congestion percentages.

2. **Run the notebook:**
   Open `final_orange.ipynb` and execute all cells sequentially.
   Example default paths and rerouted paths (via HACO) will be printed for each time step.

3. **Adjust Parameters (Optional):**

   * `ants`, `iterations` → control exploration in HACO
   * `node_penalty_weight` → adjust sensitivity to congestion
   * `high_thresh`, `low_thresh` → define congestion thresholds

---

## 🧾 Example Output

```
=== Time 30 sec ===
Server → User1: BEFORE (default) = ['Server', 'RouterB', 'User1']
             AFTER (HACO) = ['Server', 'RouterC', 'Switch5', 'User1']

RouterA at 80% → Congested
Bandwidth change: RouterA–RouterB (old=160 → new=192)
```

---

## 📊 Results

* **Reduced congestion:** dynamic rerouting balanced load across routers.
* **Improved QoS:** bandwidth adjustments prevented bottlenecks.
* **Adaptive performance:** HACO found alternative routes as congestion evolved over time.



This project is released under the MIT License.
