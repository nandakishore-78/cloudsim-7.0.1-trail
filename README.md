# Intelligent and Sustainable Cloud-based Resource Management and Scheduling

**Dissertation Project — Phase 1 Experiments**
**Author:** Nandakishore
**Framework:** CloudSim 7.0.1 | **Language:** Java | **Build Tool:** Maven

---

## Project Overview

This project investigates how cloud computing resources can be managed **intelligently and sustainably** to maximise performance while minimising waste. The core research question driving Phase 1 is:

> *How does increasing the number of virtual worker nodes affect job execution time when the workload is kept constant — and at what point does adding more infrastructure stop helping?*

Phase 1 establishes the performance baseline by simulating a cloud environment, varying the number of Virtual Machines (VMs) from 1 to 5 while keeping the workload fixed at 2 jobs. The results expose a fundamental trade-off in cloud resource management — the exact point at which scaling infrastructure no longer improves performance and instead wastes energy and cost. This is directly relevant to real-world sustainability concerns in platforms like AWS, Google Cloud, and Microsoft Azure.

---

## Why This Matters

Over-provisioning is one of the biggest inefficiencies in modern cloud computing. Data centres consume approximately 1–2% of global electricity, and a significant portion is wasted on idle compute resources. This research identifies the optimal VM-to-workload ratio — a foundational principle behind sustainable and cost-efficient cloud design.

---

## Phase 1 — Experiment Design

All four experiments share the same physical infrastructure and workload. Only the number of VMs (worker nodes) changes.

### Fixed Setup (Constant Across All Experiments)

| Component | Configuration |
|---|---|
| Physical Host | 1 CPU core, 1000 MIPS |
| Workload | 2 Cloudlets (jobs), 250,000 MI each |
| VM CPU Speed | 250 MIPS per VM |
| VM RAM | 512 MB per VM |
| Scheduling Policy | Time-Shared |

---

## Experiment Files

### `CloudSimEx1.java` — 1 VM, 2 Cloudlets (Baseline)
Both jobs share one VM. The VM splits 250 MIPS equally — each job gets only 125 MIPS.
**Expected finish time: 2000 seconds**

### `CloudSimEx3.java` — 3 VMs, 2 Cloudlets
Each job gets its own dedicated VM with full 250 MIPS. 1 VM sits idle.
**Expected finish time: 1000 seconds — 2× faster than Ex1**

### `CloudSimEx4.java` — 4 VMs, 2 Cloudlets
Same result as Ex3. 2 VMs idle — wasting 500 MIPS and 1024 MB RAM.
**Expected finish time: 1000 seconds**

### `CloudSimEx5.java` — 5 VMs, 2 Cloudlets
Same result as Ex3 and Ex4. 3 VMs idle — 75% of host MIPS wasted. Host RAM increased to 3072 MB to accommodate all 5 VMs.
**Expected finish time: 1000 seconds**

---

## Results

| Experiment | VMs | Idle VMs | MIPS per Job | Finish Time | Improvement |
|---|---|---|---|---|---|
| Ex1 | 1 | 0 | 125 MIPS | 2000 sec | baseline |
| Ex3 | 3 | 1 | 250 MIPS | 1000 sec | 2× faster |
| Ex4 | 4 | 2 | 250 MIPS | 1000 sec | 2× faster |
| Ex5 | 5 | 3 | 250 MIPS | 1000 sec | 2× faster |

```
Finish
 Time
 (sec)
 2000 |  *
      |   \
      |    \
 1000 |     *-----------*-----------*
      |
      +----+-------+-------+-------+-------
           1       3       4       5     VMs
```

Performance doubles from Ex1 to Ex3 then plateaus — proving performance is bounded by workload, not infrastructure size.

---

## Key Findings

1. **VM isolation eliminates contention and halves execution time.** Dedicated VMs per job doubled performance versus shared access.
2. **Over-provisioning has zero performance benefit.** Ex4 and Ex5 waste up to 75% of compute resources with no speed gain.
3. **Optimal node count equals number of concurrent jobs.** Beyond this, every extra VM increases cost and energy with no return.
4. **Scheduling policy determines contention behaviour.** Time-Shared reflects real-world cloud environments where workloads share physical infrastructure.

---

## How to Run

```bash
# Build
mvn clean install

# Run Experiment 1 (Baseline — 1 VM)
mvn exec:java -pl modules/cloudsim-examples/ -Dexec.mainClass=org.cloudbus.cloudsim.examples.CloudSimEx1

# Run Experiment 3 (3 VMs)
mvn exec:java -pl modules/cloudsim-examples/ -Dexec.mainClass=org.cloudbus.cloudsim.examples.CloudSimEx3

# Run Experiment 4 (4 VMs)
mvn exec:java -pl modules/cloudsim-examples/ -Dexec.mainClass=org.cloudbus.cloudsim.examples.CloudSimEx4

# Run Experiment 5 (5 VMs)
mvn exec:java -pl modules/cloudsim-examples/ -Dexec.mainClass=org.cloudbus.cloudsim.examples.CloudSimEx5
```

---

## Project Structure

```
cloudsim-7.0.1/
└── modules/
    └── cloudsim-examples/
        └── src/main/java/org/cloudbus/cloudsim/examples/
            ├── CloudSimEx1.java   ← Phase 1: Baseline — 1 VM, 2 jobs
            ├── CloudSimEx3.java   ← Phase 1: Scale — 3 VMs, 2 jobs
            ├── CloudSimEx4.java   ← Phase 1: Scale — 4 VMs, 2 jobs
            └── CloudSimEx5.java   ← Phase 1: Scale — 5 VMs, 2 jobs
```

---

## What Comes Next — Phase 2

Phase 2 will scale workload alongside VMs and explore alternative scheduling policies (SpaceShared), heterogeneous VM configurations, and intelligent scheduling algorithms for optimal VM-to-job matching.

---

---

# CloudSim: A Framework For Modeling And Simulation Of Cloud Computing Infrastructures And Services #

Cloud Computing is the leading approach for delivering reliable, secure, fault-tolerant, sustainable, and scalable computational services. Hence timely, repeatable, and controllable methodologies for performance evaluation of new cloud applications and policies before their actual development are required. Because utilization of real testbeds limits the experiments to the scale of the testbed and makes the reproduction of results an extremely difficult undertaking, simulation may be used.

CloudSim's goal is to provide a generalized and extensible simulation framework that enables modeling, simulation, and experimentation of emerging Cloud Computing infrastructures and application services, allowing its users to focus on specific system design issues that they want to investigate, without getting concerned about the low level details related to Cloud-Based infrastructures and services.

CloudSim is developed in [the Cloud Computing and Distributed Systems (CLOUDS) Laboratory](http://cloudbus.org/), at [the Computer Science and Software Engineering Department](http://www.csse.unimelb.edu.au/) of [the University of Melbourne](http://www.unimelb.edu.au/).

More information can be found on the [CloudSim's web site](http://cloudbus.org/cloudsim/).


# Main features #

  * Support for modeling and simulation of large scale Cloud Computing data centers
  * Support for modeling and simulation of virtualized server hosts, with customizable policies for provisioning host resources to Virtual Machines
  * Support for modeling and simulation of application containers
  * Support for modeling and simulation of energy-aware computational resources
  * Support for modeling and simulation of data center network topologies and message-passing applications
  * Support for modeling and simulation of federated clouds
  * Support for dynamic insertion of simulation elements, stop and resume of simulation
  * Support for user-defined policies for allocation of hosts to Virtual Machines and policies for allocation of host resources to Virtual Machines


# Download #

Either clone the repository or download a [release](https://github.com/Cloudslab/cloudsim/releases). The release package contains all the source code, examples, jars, and API html files.

# Installation
**Windows**
1) Install Java JDK21 on your system from the [official website](https://www.oracle.com/in/java/technologies/downloads/#java21) as shown in [JDK installation instructions](https://docs.oracle.com/en/java/javase/23/install/overview-jdk-installation.html)
2) Install Maven as shown on the [official website](https://maven.apache.org/install.html)
4) Compile and Run tests using the command prompt:
  ```prompt
  mvn clean install
  ```
You will find the jars in `modules/cloudsim/target/cloudsim-$VERSION.jar` and `modules/cloudsim-examples/target/cloudsim-examples-$VERSION.jar`

5) Run an example (e.g., CloudSimExample1) in cloudsim-examples using the command prompt:
```prompt
mvn exec:java -pl modules/cloudsim-examples/ -Dexec.mainClass=org.cloudbus.cloudsim.examples.CloudSimExample1
```

**Linux**
  1) Install Java JDK21 on your system:
  - On Debian-based Linux & Windows WSL2: 
    ```bash
    sudo apt install openjdk-21-jdk
    ```
  - On Red Hat-based Linux:  
    ```bash  
    sudo yum install java-21-openjdk
    ```
  2) Set Java JDK21 as default: 
  - On Debian-based Linux & Windows WSL2:
    ```bash
    sudo update-java-alternatives --set java-1.21.0-openjdk-amd64
    ```
  - On Red Hat-based Linux: 
    ```bash
    sudo update-alternatives --config 'java'
    ```
  3) Install Maven as shown on the [Official Website](https://maven.apache.org/install.html)
  4) Compile and run tests:
  ```bash
    mvn clean install
  ```
  You will find the jars in `modules/cloudsim/target/cloudsim-$VERSION.jar` and `modules/cloudsim-examples/target/cloudsim-examples-$VERSION.jar`

  5) Run an example (e.g., CloudSimExample1) in cloudsim-examples using the terminal:
  ```bash
  mvn exec:java -pl modules/cloudsim-examples/ -Dexec.mainClass=org.cloudbus.cloudsim.examples.CloudSimExample1
  ```

  **Suggestion:** Use an IDE such as IDEA Intellij to faciliate steps 4) and 5)

# Preferred Publication #
  * Remo Andreoli, Jie Zhao, Tommaso Cucinotta, and Rajkumar Buyya, [CloudSim 7G: An Integrated Toolkit for Modeling and Simulation of Future Generation Cloud Computing Environments](https://onlinelibrary.wiley.com/doi/10.1002/spe.3413), Software: Practice and Experience, 2025.
    
# Publications (Legacy) #

  * Anton Beloglazov, and Rajkumar Buyya, [Optimal Online Deterministic Algorithms and Adaptive Heuristics for Energy and Performance Efficient Dynamic Consolidation of Virtual Machines in Cloud Data Centers](http://beloglazov.info/papers/2012-optimal-algorithms-ccpe.pdf), Concurrency and Computation: Practice and Experience, Volume 24, Number 13, Pages: 1397-1420, John Wiley & Sons, Ltd, New York, USA, 2012.
  * Saurabh Kumar Garg and Rajkumar Buyya, [NetworkCloudSim: Modelling Parallel Applications in Cloud Simulations](http://www.cloudbus.org/papers/NetworkCloudSim2011.pdf), Proceedings of the 4th IEEE/ACM International Conference on Utility and Cloud Computing (UCC 2011, IEEE CS Press, USA), Melbourne, Australia, December 5-7, 2011.
  * **Rodrigo N. Calheiros, Rajiv Ranjan, Anton Beloglazov, Cesar A. F. De Rose, and Rajkumar Buyya, [CloudSim: A Toolkit for Modeling and Simulation of Cloud Computing Environments and Evaluation of Resource Provisioning Algorithms](http://www.buyya.com/papers/CloudSim2010.pdf), Software: Practice and Experience (SPE), Volume 41, Number 1, Pages: 23-50, ISSN: 0038-0644, Wiley Press, New York, USA, January, 2011. (Seminal paper)**
  * Bhathiya Wickremasinghe, Rodrigo N. Calheiros, Rajkumar Buyya, [CloudAnalyst: A CloudSim-based Visual Modeller for Analysing Cloud Computing Environments and Applications](http://www.cloudbus.org/papers/CloudAnalyst-AINA2010.pdf), Proceedings of the 24th International Conference on Advanced Information Networking and Applications (AINA 2010), Perth, Australia, April 20-23, 2010.
  * Rajkumar Buyya, Rajiv Ranjan and Rodrigo N. Calheiros, [Modeling and Simulation of Scalable Cloud Computing Environments and the CloudSim Toolkit: Challenges and Opportunities](http://www.cloudbus.org/papers/CloudSim-HPCS2009.pdf), Proceedings of the 7th High Performance Computing and Simulation Conference (HPCS 2009, ISBN: 978-1-4244-4907-1, IEEE Press, New York, USA), Leipzig, Germany, June 21-24, 2009.




[![](http://www.cloudbus.org/logo/cloudbuslogo-v5a.png)](http://cloudbus.org/)
