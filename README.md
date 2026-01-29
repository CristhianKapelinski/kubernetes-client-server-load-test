# Kubernetes-client-server-load-test

This project automates performance tests between server and client implementations in Go and C++ using Kubernetes. The goal is to compare the performance of both languages in different load scenarios by collecting metrics, processing results, and generating analysis graphs and reports.

## Features

- **Automated build and push** of Docker images for servers and clients (Go and C++).
- **Automated execution** of multiple test scenarios, varying:
    - Number of server replicas
    - Number of concurrent clients
    - Number of messages per client
- **Kubernetes Orchestration**: each scenario runs as an isolated Job.
- **Log collection** from clients for later analysis.
- **Log processing** into CSV via Python.
- **Automatic generation** of graphs and **analysis reports**.
- **Repeated execution** of tests for greater statistical robustness.
- **Automatic cleanup** of Kubernetes resources at the end of each cycle.

## Prerequisites

- Docker and Docker Hub login
- Kubernetes (configured `kubectl`)
- Python 3 (all necessary dependencies are already in the repository)
- Permission to create and delete resources in the Kubernetes cluster

## How to use

1. **Clone the repository and access the directory:**
      ```bash
      git clone [https://github.com/CristhianKapelinski/kubernetes-client-server-load-test.git](https://github.com/CristhianKapelinski/kubernetes-client-server-load-test.git)
      cd kubernetes-client-server-load-test/v4
      ```

2. **Edit configurations if necessary:**
      - Docker Hub User (`DOCKER_USER`)
      - Test parameters (replicas, concurrency, messages)

3. **Run the main script:**
      ```bash
      ./run_tests.sh [options]
      ```

      **Optional parameters:**
      - `-u <docker_user>`: Docker Hub User
      - `-s <server_replicas>`: List of server replicas (e.g., 2,4,6)
      - `-c <client_concurrency>`: List of concurrent clients (e.g., 10,20,30)
      - `-m <messages_per_client>`: List of messages per client (e.g., 1,10,100)

      **Example:**
      ```bash
      ./run_tests.sh -u your_user -s 2,4 -c 10,20 -m 1,10
      ```

## Results

- Logs, CSVs, graphs, and reports are saved in the `logs/` directory.
- At the end, aggregated graphs and analyses are generated automatically.

## Test Structure

For each language (Go and C++), the script:
- Builds and pushes Docker images.
- Executes multiple scenarios varying replicas, concurrency, and messages.
- Collects and processes client logs.
- Generates CSVs, graphs, and reports.
- All Kubernetes resources are cleaned up between runs to ensure isolation.

## Auxiliary Scripts

All necessary scripts are already present in the repository:
- `process_logs.py`: Processes raw client logs and generates CSV.
- `generate_graphs.py`: Generates graphs from the CSVs.
- `analyze_results.py`: Generates result analysis reports.

## Notes

- The script is robust against failures and automatically times out problematic jobs.
- The environment is cleaned at the end of each cycle and upon script completion.
- Parameters can be adjusted according to the experiment's needs.
- **Attention:** Ensure your Kubernetes cluster has sufficient resources for the tests and that you are logged into Docker Hub.

## Generated Graphs

The main analysis graphs for the tests are available below:

- ![Latency vs Clients (with outliers)](v4/logs/final_graphs/1a_latency_vs_clients_with_outliers.png)
- ![Latency vs Clients (no outliers)](v4/logs/final_graphs/1b_latency_vs_clients_no_outliers.png)
- ![Latency vs Server Replicas (with outliers)](v4/logs/final_graphs/3a_latency_vs_servers_with_outliers.png)
- ![Latency vs Server Replicas (no outliers)](v4/logs/final_graphs/3b_latency_vs_servers_no_outliers.png)
- ![Overall Latency Distribution](v4/logs/final_graphs/4_overall_latency_distribution.png)
- ![Latency Heatmap (C++)](v4/logs/final_graphs/5_heatmap_latency_cpp.png)
- ![Latency Heatmap (Go)](v4/logs/final_graphs/5_heatmap_latency_go.png)
- ![Latency Stability per Run - Scenario 1](v4/logs/final_graphs/6a_run_stability_latency_scenario1.png)
- ![Latency Stability per Run - Scenario 3](v4/logs/final_graphs/6a_run_stability_latency_scenario3.png)
---

## General Statistics Grouped by Language


**Table 1 General Statistics of the 10 runs:**

| Language | Avg Latency (ms) | Standard Deviation | Median  | 95th Percentile | Max | Success Rate (%) | Total Messages Received (avg) |
|----------|------------------|--------------------|---------|-----------------|-----|------------------|-------------------------------|
| cpp      | 1.52             | 0.73               | 1.40    | 2.75            | 3.57| 100              | 106434.17                     |
| go       | 0.95             | 0.44               | 0.97    | 1.64            | 2.32| 100              | 106434.17                     |


**Table 2 Execution number 10:**

| Language  | Avg Latency (ms) | Standard Deviation | Median  | 95th Percentile | Max | Success Rate (%) | Total Messages Received (avg) |
|-----------|------------------|--------------------|---------|-----------------|-----|------------------|-------------------------------|
| C++       | 0.51             | 0.31               | 0.43    | 1.02            | 2.66| 100              | 106434.17                     |
| Go        | 0.34             | 0.17               | 0.32    | 0.66            | 1.10| 100              | 106434.17                     |
---

## Comparison and Interpretation of Tables

Upon comparing the two tables, which present consolidated statistics by language, we observe some consistent trends and some notable differences in metric values.

**Consistencies:**

* **Total Messages Received (avg):** Both tables show that, on average, both the C++ and Go implementations processed the same volume of messages: **106,434.17 messages**. This suggests that the applied test load was the same for both languages and that the system was able to process the load consistently.
* **Performance Hierarchy (C++ vs. Go):** In both tables, the **Go implementation consistently presents lower latencies** (Mean, Median, 95th Percentile, and Max) and a lower standard deviation compared to the C++ implementation. This indicates that Go is faster and more consistent in its response time than C++.

**Differences and Discrepancies:**

---
