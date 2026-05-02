# 🚗 Car Wash Service Station Simulator

Developed in collaboration with @minshawi0 as part of the Cryptography course at Cairo University, Faculty of Engineering.

A multithreaded Java simulation of a car wash service station with a real-time GUI.  
Built using Java Threads, custom Semaphores, and Java Swing — no external libraries required.

---

## 📌 Features

- Real-time graphical interface built with Java Swing
- Custom Semaphore implementation for thread synchronization
- Multithreaded simulation: cars and pumps run as independent threads
- Configurable waiting area capacity, number of pumps, and number of cars
- Live event log showing every arrival, queue entry, and service update
- Color-coded pump status (🟢 Free / 🟠 Busy)

---

## 🚀 How It Works

| Component | Role |
|-----------|------|
| **Car** | A thread that arrives, joins the waiting queue, and waits for a pump |
| **Pump** | A thread that picks cars from the queue and simulates washing (3.5s) |
| **Semaphore (mutex)** | Protects shared queue from race conditions |
| **Semaphore (empty)** | Tracks available spots in the waiting area |
| **Semaphore (full)** | Signals pumps when a car is ready |
| **Semaphore (pumpLimit)** | Limits how many pumps can be active at once |

---

## 🖥️ GUI Overview

```
┌─────────────────────────────────────────────────────┐
│  Waiting Area Capacity: [5]   Number of Pumps: [2]  │
│  Number of Cars:        [5]   [Start Simulation]    │
├───────────────────────────┬─────────────────────────┤
│  Pump 1: C1 (Busy) 🟠    │  [ARRIVAL] C1 arrived   │
│  Pump 2: Free 🟢          │  [QUEUE] C2 entered...  │
│                           │  [SERVICE START] C1...  │
├───────────────────────────┴─────────────────────────┤
│            Waiting Queue: 3 cars                    │
└─────────────────────────────────────────────────────┘
```

---

## 🛠️ How to Run

### Requirements
- Java JDK 8 or higher

### Compile
```bash
javac ServiceStation.java
```

### Run
```bash
java ServiceStation
```

The GUI window will open. Enter your settings and click **Start Simulation**.

---

## ⚙️ Simulation Settings

| Setting | Description | Default |
|---------|-------------|---------|
| Waiting Area Capacity | Max cars that can wait in queue | 5 |
| Number of Pumps | How many wash stations run in parallel | 2 |
| Number of Cars | Total cars that will arrive | 5 |

---

## 📁 Project Structure

```
Car-Wash-Simulator/
│
├── ServiceStation.java     # Main source file (Car, Pump, Semaphore, GUI)
└── README.md               # Project documentation
```

---

## 💡 Example Log Output

```
[ARRIVAL] C1 arrived at the garage.
[QUEUE] C1 entered the waiting queue. (Queue size: 1)
[TAKEN] Pump 1 took C1 for service. (Queue size: 0)
[SERVICE START] C1 started service at Pump 1 (Acquiring Bay)
[ARRIVAL] C2 arrived at the garage.
[QUEUE] C2 entered the waiting queue. (Queue size: 1)
[TAKEN] Pump 2 took C2 for service. (Queue size: 0)
[SERVICE START] C2 started service at Pump 2 (Acquiring Bay)
[SERVICE END] C1 finished service at Pump 1 (Releasing Bay)
[SERVICE END] C2 finished service at Pump 2 (Releasing Bay)
```

---

## 🧠 Concurrency Design

### Problem Being Solved
Multiple cars arrive at a car wash with a limited waiting area and limited pumps.  
The simulation must ensure:
- No two threads access the queue at the same time (**mutual exclusion**)
- Cars wait if the waiting area is full (**bounded buffer**)
- Pumps wait if no car is available (**producer-consumer**)

### Solution
This is a classic **Producer-Consumer** problem solved using three semaphores:

```
mutex     → ensures only one thread touches the queue at a time
empty     → counts free slots in the waiting area (cars wait if 0)
full      → counts cars ready to be served (pumps wait if 0)
pumpLimit → limits concurrent pump usage
```

---

## 👨‍💻 Author

Made with ❤️ in Java.  
Feel free to fork, star ⭐, and contribute!

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
