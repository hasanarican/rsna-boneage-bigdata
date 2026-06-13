# RSNA Bone Age — End-to-End Big Data Pipeline
## MSc Data Analytics | Big Data Analytics Assignment
### Berlin School of Business & Innovation

## Project Overview

This project implements a complete end-to-end Big Data pipeline for automated bone age estimation from paediatric hand X-ray metadata. The pipeline demonstrates the full lifecycle of a Big Data system: distributed storage (Hadoop HDFS), SQL-based exploration (Apache Hive), large-scale data processing (Apache Spark/PySpark), and machine learning (Scikit-learn).

**Use Case:** Automated bone age estimation to assist radiologists in paediatric diagnostics, reducing inter-observer variability and diagnostic workload.

## Dataset

- **Source:** RSNA Bone Age Dataset — https://www.kaggle.com/datasets/kmader/rsna-bone-age
- **Size:** 9.97 GB
- **Records:** 12,611 training samples
- **Features:** Patient ID, bone age (months), biological sex
- **Age Range:** 1 to 228 months

## Technology Stack

| Technology | Version | Role |
|------------|---------|------|
| Docker | 24.0.7 | Containerised environment |
| Docker Compose | v2.23.3 | Multi-container orchestration |
| Hadoop HDFS | 3.2.1 | Distributed file storage |
| Apache Hive | 2.3.2 | SQL-based data warehouse |
| Apache Spark (PySpark) | 3.1.1 | Distributed data processing |
| Scikit-learn | Latest | Machine learning models |
| Jupyter Notebook | PySpark kernel | Interactive development |

## Project Structure
rsna-boneage-bigdata/

├── docker-compose.yml          # Orchestrates all Big Data containers

├── hadoop.env                  # Hadoop environment configuration

├── 01_data_ingestion.ipynb     # Task 3: Spark wrangling + HiveQL queries

├── 02_machine_learning.ipynb   # Task 4: ML model training and evaluation

└── README.md                   # Setup and replication instructions

## Environment Setup

### Prerequisites
- Docker Desktop (v24+) with at least 10 GB free disk space
- Docker Compose (v2+)
- Minimum 8 GB RAM recommended
- Dataset downloaded from Kaggle (boneage-training-dataset.csv)

### Step 1: Start All Containers
docker-compose up -d

### Step 2: Verify Containers Are Running
docker ps

### Step 3: Access Services

| Service | URL |
|---------|-----|
| JupyterLab | http://localhost:8888 |
| Spark Master UI | http://localhost:8080 |
| HDFS NameNode UI | http://localhost:9870 |
| YARN Resource Manager | http://localhost:8088 |

### Step 4: Upload Dataset
1. Open JupyterLab at http://localhost:8888
2. Upload boneage-training-dataset.csv to the /work directory

### Step 5: Run Notebooks
1. Open 01_data_ingestion.ipynb → Run All Cells
2. Open 02_machine_learning.ipynb → Run All Cells

## Results

| Model | MAE (months) | RMSE (months) | R2 Score |
|-------|-------------|---------------|----------|
| Random Forest | 32.42 | 40.14 | 0.0586 |
| Gradient Boosting | 32.43 | 40.14 | 0.0586 |

**Best Model:** Random Forest (MAE = 32.42 months)

## Stop the Environment
docker-compose down

## Author
Hasan Arican | MSc Data Analytics | Berlin School of Business & Innovation | 2025
