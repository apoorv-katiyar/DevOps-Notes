# **DORA Metrics Documentation**

## **Introduction**

DORA (DevOps Research and Assessment) metrics help organizations measure and improve their DevOps performance. These metrics provide insights into software delivery efficiency and reliability.

The four key DORA metrics are:

1. **Deployment Frequency (DF)** - How often deployments occur.
2. **Lead Time for Changes (LT)** - Time from commit to deployment.
3. **Change Failure Rate (CFR)** - Percentage of failed deployments.
4. **Mean Time to Recovery (MTTR)** - Time taken to recover from failures.

## **The places from where we can fetch DORA Metrics**

In an Travelers org, multiple tools are used for development, CI/CD, and incident management. We can extract DORA metrics from the following sources:

| **Metric**            | **Tools**                           |
| --------------------- | ----------------------------------- |
| Deployment Frequency  | GitHub, GitLab, Jenkins, ServiceNow |
| Lead Time for Changes | GitHub, GitLab, Jenkins, ServiceNow |
| Change Failure Rate   | GitHub, GitLab, Jenkins, ServiceNow |
| Mean Time to Recovery | GitHub, GitLab, Jenkins, ServiceNow |

---

## **1️⃣ Deployment Frequency (DF)**

**Definition:** Measures about the frequency of deployments happen successfully.

### **We can extract data from the following tools**

| **Tool**   | **Data to Fetch**                                         | **API's Endpoint**                                               |
| ---------- | --------------------------------------------------------- | ---------------------------------------------------------------- |
| GitHub     | Successful deployments from GitHub Actions                | `/repos/{owner}/{repo}/actions/runs?status=success`              |
| GitLab     | Successful CI/CD pipelines                                | `/projects/:id/pipelines?status=success`                         |
| Jenkins    | Successful pipeline runs                                  | `/job/{job-name}/lastBuild/api/json`                             |
| ServiceNow | Closed Change Requests we can categorized into deployment | `/api/now/table/change_request?category=Deployment^state=Closed` |

---

## **2️⃣ Lead Time for Changes (LT)**

**Definition:** Measures the time from code commit to successful into production.

### **We can extract data from the following tools**

| **Tool**   | **Data to Fetch**                              | **API's Endpoint**                                                   |
| ---------- | ---------------------------------------------- | -------------------------------------------------------------------- |
| GitHub     | PR created → merged → deployed time            | `/repos/{owner}/{repo}/pulls?state=closed`                           |
| GitLab     | Merge Requests merged & deployed               | `/projects/:id/merge_requests?state=merged`                          |
| Jenkins    | Job trigger time to completion time            | `/job/{job-name}/builds`                                             |
| ServiceNow | Code Change Request approval → deployment time | `/api/now/table/change_request?category=Code Change^state=Completed` |

---

## **3️⃣ Change Failure Rate (CFR)**

**Definition:** Percentage of deployments that lead to failure and need fixes.

### **We can extract data from the following tools**

| **Tool**   | **Data to Fetch**                      | **API's Endpoint**                                               |
| ---------- | -------------------------------------- | ---------------------------------------------------------------- |
| GitHub     | Failed GitHub Actions runs             | `/repos/{owner}/{repo}/actions/runs?status=failure`              |
| GitLab     | Failed CI/CD pipelines                 | `/projects/:id/pipelines?status=failed`                          |
| Jenkins    | Failed build jobs                      | `/job/{job-name}/lastBuild/api/json`                             |
| ServiceNow | Failed deployments linked to incidents | `/api/now/table/change_request?category=Deployment^state=Failed` |

---

## **4️⃣ Mean Time to Recovery (MTTR)**

**Definition:** Time taken to recover from a failure after deployment.

### **We can extract data from the following tools**

| **Tool**   | **Data to Fetch**                          | **API's Endpoint**                                                                 |
| ---------- | ------------------------------------------ | ---------------------------------------------------------------------------------- |
| GitHub     | Failed → next successful deployment        | Compare timestamps from `/repos/{owner}/{repo}/actions/runs`                       |
| GitLab     | Failed pipeline → next successful pipeline | `/projects/:id/pipelines?status=failed` → `/projects/:id/pipelines?status=success` |
| Jenkins    | Failed job → next successful build         | `/job/{job-name}/builds`                                                           |
| ServiceNow | Incident open time → resolution time       | `/api/now/table/incident?state=Resolved`                                           |

---

The architecture involves integrating various tools (GitHub, GitLab, Jenkins, ServiceNow) with a central data processing system that fetches, processes, and stores DORA metrics.

Components:

Data Sources: GitHub, GitLab, Jenkins, ServiceNow
Data Fetching Layer: APIs to fetch data from the tools
Data Processing Layer: Processes raw data to calculate DORA metrics
Data Storage: Database to store processed metrics
Visualization Layer: Dashboard to visualize DORA metrics


Architecture Diagram:

Explanation:

Data Sources from where we will be fetching the data: Tools like GitHub, GitLab, Jenkins, and ServiceNow provide data through their APIs.
Data Fetching Layer: This layer consists of scripts or services that periodically call the APIs to fetch the required data.
Data Processing Layer: This layer processes the raw data to calculate the DORA metrics.
Data Storage Layer: The processed metrics are stored in a database for further analysis and visualization.
Visualization Layer: A dashboard that visualizes the DORA metrics, providing insights into the DevOps performance.

+----------------+       +-------------------+       +-------------------+       +-------------------+
|                |       |                   |       |                   |       |                   |
|    GitHub      +------>+                   |       |                   |       |                   |
|                |       |                   |       |                   |       |                   |
+----------------+       |                   |       |                   |       |                   |
                         |                   |       |                   |       |                   |
+----------------+       |                   |       |                   |       |                   |
|                |       |                   |       |                   |       |                   |
|    GitLab      +------>+ Data Fetch        +------>+ Data Processing   +------>+ Data Storage      |
|                |       |                   |       |                   |       |                   |
+----------------+       |                   |       |                   |       |                   |
                         |                   |       |                   |       |                   |
+----------------+       |                   |       |                   |       |                   |
|                |       |                   |       |                   |       |                   |
|    Jenkins     +------>+                   |       |                   |       |                   |
|                |       |                   |       |                   |       |                   |
+----------------+       +-------------------+       +-------------------+       +-------------------+
                         |                   |       |                   |       |                   |
+----------------+       |                   |       |                   |       |                   |
|                |       |                   |       |                   |       |                   |
|  ServiceNow    +------>+                   |       |                   |       |                   |
|                |       |                   |       |                   |       |                   |
+----------------+       +-------------------+       +-------------------+       +-------------------+

+-----------------------+
|                       |
| (Dashboard)-> Grafana |               
|                       |
+-----------------------+
