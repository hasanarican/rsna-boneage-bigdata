# RSNA Bone Age — End-to-End Big Data Pipeline
## MSc Data Analytics | Big Data Analytics Assignment
### Berlin School of Business & Innovation

---

## Project Overview

This project implements a complete end-to-end Big Data pipeline for automated bone age estimation from paediatric hand X-ray metadata. The pipeline demonstrates the full lifecycle of a Big Data system: distributed storage (Hadoop HDFS), SQL-based exploration (Apache Hive), large-scale data processing (Apache Spark/PySpark), and machine learning (Scikit-learn).

**Use Case:** Automated bone age estimation to assist radiologists in paediatric diagnostics, reducing inter-observer variability and diagnostic workload.

---

## Dataset

- **Source:** RSNA Bone Age Dataset — https://www.kaggle.com/datasets/kmader/rsna-bone-age
- **Size:** 9.97 GB
- **Records:** 12,611 training samples
- **Features:** Patient ID, bone age (months), biological sex
- **Age Range:** 1 to 228 months
- **Gender Split:** 6,833 Male | 5,778 Female

---

## Technology Stack

| Technology | Version | Role |
|------------|---------|------|
| Docker | 24.0.7 | Containerised environment |
| Docker Compose | v2.23.3 | Multi-container orchestration |
| Hadoop HDFS | 3.2.1 | Distributed file storage |
| Apache Hive | 2.3.2 | SQL-based data warehouse |
| Apache Spark (PySpark) | 3.1.1 | Distributed data processing |
| Scikit-learn | Latest | ML models (RF, GB, KNN) |
| Jupyter Notebook | PySpark kernel | Interactive development |

---

## Project Structure

```
rsna-boneage-bigdata/
├── docker-compose.yml              # Orchestrates all 10 Big Data containers
├── hadoop.env                      # Hadoop environment configuration
├── 01_data_ingestion.ipynb         # Task 3: Spark wrangling + HiveQL queries
├── 02_machine_learning.ipynb       # Task 4: ML model training and evaluation
├── eda_plots.png                   # Exploratory Data Analysis visualisations
├── feature_importance.png          # Random Forest feature importance
├── model_evaluation.png            # Model comparison plots
├── residual_analysis.png           # Residual analysis plots
└── README.md                       # Setup and replication instructions
```

---

## Environment Setup

### Prerequisites
- Docker Desktop (v24+) with at least 10 GB free disk space on the Docker drive
- Docker Compose (v2+)
- Minimum 8 GB RAM
- Dataset downloaded from Kaggle (boneage-training-dataset.csv)

### Step 1: Start All Containers
```bash
docker-compose up -d
```

This starts 10 containers: namenode, datanode, resourcemanager, nodemanager, hive-metastore, hive-server, spark-master, spark-worker, spark-history, and jupyter.

### Step 2: Verify Containers
```bash
docker ps
```

### Step 3: Access Services

| Service | URL |
|---------|-----|
| JupyterLab | http://localhost:8888 |
| Spark Master UI | http://localhost:8080 |
| HDFS NameNode UI | http://localhost:9870 |
| YARN Resource Manager | http://localhost:8088 |
| Hive Web UI | http://localhost:10002 |

### Step 4: Upload Dataset
1. Open JupyterLab at http://localhost:8888
2. Click the Upload button (↑) in the left panel
3. Upload `boneage-training-dataset.csv` to the `/work` directory

### Step 5: Run Notebooks (in order)
1. Open `01_data_ingestion.ipynb` → **Run → Run All Cells**
2. Open `02_machine_learning.ipynb` → **Run → Run All Cells**

---

## Results

### Data Processing (Task 3)
- **Total Records:** 12,611 (zero null values)
- **Gender:** 6,833 Male | 5,778 Female
- **Mean Bone Age:** 127.32 months (SD: 41.18)
- **Male avg:** 135.30 months | **Female avg:** 117.88 months
- **Spark execution time:** 0.807 seconds
- **HiveQL execution time:** 0.978 seconds

### Machine Learning (Task 4)

| Model | MAE (months) | RMSE (months) | R2 Score |
|-------|-------------|---------------|----------|
| Random Forest | 32.24 | 40.29 | 0.0517 |
| Gradient Boosting | **32.12** | **40.03** | **0.0636** |
| KNN Regressor | 33.09 | 41.41 | -0.0019 |

**Best Model:** Gradient Boosting (MAE = 32.12 months)

**Note on features:** Data leakage was prevented by excluding boneage_years (a direct transformation of the target) from the feature set. Features used: gender_binary, id_normalized, gender_x_id.

---

## Stop the Environment
```bash
docker-compose down
```

---

## Author
**Hasan Arican** | MSc Data Analytics | Berlin School of Business & Innovation | 2026
