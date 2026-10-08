# ☀️ Helios-OS: Smart Building & Neighborhood Renewable OS Controller

![Core Engine](https://img.shields.io/badge/Kernel_Engine-C_(GCC)-00599C?style=for-the-badge&logo=c)
![Command Center](https://img.shields.io/badge/Command_Center-Streamlit_UI-FF4B4B?style=for-the-badge&logo=streamlit)
![Grid Safety](https://img.shields.io/badge/MCB_Guard-100%25_SAFE_(0_Trips)-10B981?style=for-the-badge)
![Energy Efficiency](https://img.shields.io/badge/Clean_Energy-Zero_Solar_Wasted-F59E0B?style=for-the-badge)

> **Intelligent Operating System Resource Management for Solar, Wind, and Inverter-Powered Smart Buildings.**

---

## ⚡ The Problem: The Inverter & Geyser Overload

During a power cut at home, when the house is running on the backup inverter, someone inevitably turns on a high-wattage **geyser, microwave, or air conditioner**. Within seconds, the inverter starts beeping frantically, overloads, and trips the **MCB (Miniature Circuit Breaker)**—plunging the entire house into darkness.

Now scale that exact problem to an entire **smart building or neighborhood** running on **rooftop solar panels, wind turbines, and battery inverters**:
* **When a cloud passes over or the wind dies down:** If multiple flats turn on their ACs, water motors, and washing machines simultaneously without checking live renewable supply, the central inverter overloads and crashes the building grid.
* **When the sun is blazing:** If heavy appliances sit idle while rooftop solar generation peaks, free clean electricity goes completely to waste.

Instead of relying on expensive proprietary hardware to fix this, **Helios-OS** solves the problem in software using core **Operating System Kernel rules**. Just as Windows, Linux, or Android decides which app gets processor time, memory buffers, and disk access on a smartphone without draining the battery or crashing the OS, **Helios-OS** watches solar panels, wind turbines, and batteries every second, places appliance power requests into a smart queue, and feeds electricity to the right loads at the right time—so the **MCB never trips** and **zero clean energy is wasted**.

---

## 🧠 How Helios-OS Works (Core Kernel Architecture)

Helios-OS translates classic Operating System resource management directly into smart-grid electrical control across four core subsystems:

### 1. Adaptive Smart Appliance Queue (Priority vs. Round Robin Scheduling)
Helios-OS continuously monitors incoming clean power ($P_{\text{total}} = P_{\text{solar}} + P_{\text{wind}}$) against the building's safe inverter threshold (`50 kW`):
* **☀️ Blazing Sun & Wind Surplus Mode ($\ge 50\text{ kW}$):** Engages **Priority Scheduling**. Critical infrastructure and heavy appliances (`Emergency_Lift_Lights`, `Building_Water_Motor`, `Washing_Machine_Bank`, `Split_AC_Cluster`, `Geyser_Microwave_Load`) are scheduled immediately according to priority so **100% of free solar and wind power is utilized**.
* **☁️ Cloud Cover / Inverter Backup Mode ($< 50\text{ kW}$):** Automatically switches to **Round Robin Scheduling (`Time Quantum = 3 mins`)**. Instead of letting a single heavy geyser or AC hog the inverter and trip the breaker, appliances take controlled 3-minute turns—preventing continuous peak current draw and keeping the inverter silent and stable.

### 2. Inverter Overload & MCB Trip Guard (Banker's Deadlock Avoidance)
Before energizing competing high-wattage building circuits, Helios-OS models available power across three resource buses:
* `R0`: Rooftop Solar Panels (`kW-blocks`)
* `R1`: Wind Turbines (`kW-blocks`)
* `R2`: Inverter Battery Bank (`kWh-blocks`)

Using **Banker's Safety Algorithm**, Helios-OS compares each circuit's `CurrentDraw` and `MaxPeak` against real-time `Available` clean power to compute a mathematically guaranteed **Safe Appliance Switching Sequence**, ensuring the building never enters an overload deadlock.

### 3. Zero-Drain Inverter Buffer Allocator (Best-Fit Memory Management)
Smart-grid controllers process continuous telemetry streams from solar inverters, AC clusters, geyser queues, and motor relays. Helios-OS uses **Best-Fit Fixed Partition Allocation** to pack incoming data streams into the tightest matching memory buffer slots. By consolidating active buffers into the minimum number of partitions, unused memory slots remain completely unallocated and enter **Zero-Drain Standby Sleep (`0 W`)**.

### 4. Low-Joule Smart-Meter Logger (C-LOOK Disk I/O Scheduling)
Recording high-frequency smart-meter readings across dozens of apartments generates heavy storage I/O. Rather than servicing log requests in an erratic First-Come, First-Served (`FCFS`) order, Helios-OS orders disk head movement using **C-LOOK**—sweeping unidirectionally across storage cylinders to cut mechanical seek distance and storage controller energy consumption in half.

---

## 📊 System Benchmarks & Verified Telemetry

### Adaptive Appliance Scheduling Performance
| Metric | Blazing Sun Mode (`55 kW`) | Cloud Cover Mode (`40 kW`) | System Impact |
| :--- | :--- | :--- | :--- |
| **Active Scheduling Engine** | **Priority Queue** | **Round Robin (`Q = 3 mins`)** | Automatic real-time switch at `50 kW` threshold |
| **Execution Sequence** | `P101 → P102 → P105 → P103 → P104` | `P101 → P102 → P103 → P104 → P105 → ...` | Time-slicing prevents sustained peak inverter draw |
| **Average Waiting Time (AWT)** | **`10.40 mins`** | **`15.40 mins`** | Trades `5.00 mins` wait time during cloud cover to prevent blackouts |
| **Average Turnaround Time (TAT)** | **`15.60 mins`** | **`20.60 mins`** | All 5 building loads finish within `26 mins` with zero starvation |
| **Clean Energy Utilized** | **`7.95 kWh` (Zero Waste)** | **`7.95 kWh` (MCB: SAFE)** | 0 MCB trips; 100% renewable absorption |

### Banker's Algorithm MCB Protection Audit
* **Monitored Circuits:** `P0 (Emergency Lift & Corridor)`, `P1 (Water Motor Pump Station)`, `P2 (Central Split-AC Floor)`, `P3 (Geyser & Microwave Line)`, `P4 (Washing Machine & EV Bay)`
* **Verified Safe Switching Sequence:**
  ```text
  P1 -> P3 -> P4 -> P0 -> P2