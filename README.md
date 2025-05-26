# [<img src="src/web-ui/public/images/drp_logo_blue.png" width="250"/>](src/web-ui/public/images/drp_logo_blue.png)
🧠 Deep Research Pipeline  
## Overview
A modular, fully local, open-source pipeline for fetching, structuring, and exploring AI research papers from arXiv.org. This system enables offline graph-based and semantic search through an integrated architecture of MongoDB, Neo4j, and Qdrant using Hugging Face embeddings. All services run in Docker containers for easy, consistent local deployment.

## 🚀 Key Features
| Feature                  | Description |
| ------------------------ |-------------|
| 🏠 **Local-first** | Everything runs offline with no cloud dependencies on local network with multi machine support. |
| 💾 **ArXiv Ingestion** | Fetches non-duplicate papers from configurable categories (e.g., cs.AI) with smart date filtering. |
| 🗒 **MongoDB Storage** | Stores structured metadata, paper information, and download statuses. |
| 🐙 **Graph Representation** | Neo4j graph database captures relationships between papers, authors, and categories. |
| 🤖 **LLM Category Summary** | Uses LLMs to categorize papers by subject, architecture, and mathematical models. |
| 💡 **Semantic Embeddings** | Creates vector embeddings using Hugging Face models, stored in Qdrant for similarity search. |
| 🔧 **Configurable & Modular** | Centralized settings allow switching categories, models, and components. |
| 👀 **User Interface** | User-friendly interface for exploring datasets, knowledge graphs, and similarity search. |
| 📦 **Containerized** | Mostly Dockerized with persistent volumes for reliable data storage and consistent execution. |
---

### Neo4j Graph Database

![Image](https://github.com/user-attachments/assets/3233595b-ecbc-4029-a0f9-1e6723c026a7)
### Dashboard reporting
![alt text](<images/Screenshot 2025-05-09 013100.png>)
### Qdrant Vector Database
![alt text](<images/Screenshot 2025-05-09 012044.png>)
### Qdrant Vector Database similarity search
![alt text](<images/Screenshot 2025-05-09 010713.png>)
---

## 📦 System Components
| Component                  | Purpose                                      |
| -------------------------- | -------------------------------------------- |
| **Ingestion Service**      | Fetches papers(metadata) using arXiv Atom XML API      |
| **MongoDB**                | Stores normalized paper metadata and paper processing tracking    |
| **Neo4j**                  | Stores the author-paper-category graph       |
| **Qdrant**                 | Stores paper vector embeddings for semantic search with metrics tracking |
| **Config Manager**         | Central config for category, limits, model   |
| **User Interface**         | Web UI for interaction with graphs           |
| **Logger**                 | Tracks events, errors, and skipped entries   |
| **Docker Compose**         | Brings it all together for local use         |
| **Prometheus**             | Time series database for system metrics collection  |
| **Grafana**                | Visualization platform for system metrics dashboards |
| **Kafka**                  | Distributed event streaming platform for messaging |
| **Zookeeper**              | Coordinates the Kafka cluster                |
| **Kafka UI**               | Web interface for Kafka management and monitoring |

---
## 🧵 High Level Overview
 - Fetch metadata of papers from arXiv.org using arXiv Atom XML API
 - Store normalized metadata in MongoDB with pdf_url for pdf download
 - Download PDFs from arXiv.org and store in local directory
 - Store the author-paper-category graph in Neo4j
 - Store vector embeddings for semantic search in Qdrant
 - Manage paper processing tracking in MongoDB
 - Dynamic configuration for paper category, paper limits, models, and pdf save directory
 - Web UI for interaction with graphs
 - Jupyter notebook for interactive analysis
 - Tracks events, errors, and skipped entries  

For more deep dive into project and status, see the `docs/` directory.

---

## 💡 Use Cases
### Research & Knowledge Management
- **Build Personal Research Libraries**: Create customized collections of AI papers organized by category and relevance
- **Offline Semantic Paper Search**: Find relevant papers without relying on online search engines
- **Research Gap Identification**: Analyze research areas to identify unexplored topics and opportunities
- **Literature Review Automation**: Quickly build comprehensive literature reviews for specific research questions

### Data Science & Analysis
- **Research Trend Analysis**: Apply time-series analysis to identify emerging and declining research topics
- **Citation Impact Visualization**: Build network graphs to identify the most influential papers and authors
- **Cross-Domain Knowledge Transfer**: Discover applications of techniques across different research domains
- **Research Benchmarking**: Track performance improvements in specific algorithms or methods over time

### AI-Assisted Research
- **Paper Summarization**: Generate concise summaries of complex research papers
- **Similar Papers Discovery**: Use vector similarity to find related work not linked by citations
- **Research Idea Generation**: Use paper combinations with LLMs to explore novel research directions
- **Algorithm Implementation Assistance**: Extract mathematical models for implementation in your own projects
- **Research Agent**: Add specific research agents for specific use cases
- **Fine-tuning**: Fine-tune pipelines for specific use cases

### Education & Learning
- **Personalized Learning Paths**: Create sequential reading lists for specific AI topics
- **Concept Visualization**: Extract and visualize key concepts across multiple papers
- **Interactive Research Exploration**: Navigate research spaces through concept and citation graphs
- **Teaching Material Preparation**: Curate papers and extract examples for courses and tutorials
---

# 🛠️ Setup Instructions
  ### This system runs on a single machine but recommend a multiple machine setup.*
  - System minimum requirements: 16GB RAM, 8GB GPU, 512GB SSD
  - Developer runs on laptop with 16GB RAM, 16GB GPU, 1TB SSD *Not recommended!
  - Most components run in docker containers that can be move to own/shared docker machines
  - Qdrant recommend setup on a separate machine with nvidia GPU for faster vector operations
  - Default Qdrant running locally with out docker *see below Qdrant Setup*
  - External storage for PDFs recommended *see below PDF Storage*
  * pdf save directory is set to E:\AI Research\ in "config/default.yaml"
  * edit config/default.yaml before running the pipelines
  * Project works on both Windows and Ubuntu/Linux environments.

---

# ⚠️ Prerequisites
- Git
- Python 3.9+ (Python 3.11 recommended)
- [UV](https://github.com/astral-sh/uv) (for fast Python dependency management)
- [Ollama](https://ollama.ai/) (optional, for enhanced image analysis)
- Docker and Docker Compose (for containerized deployment)
- NVIDIA GPU with CUDA support (optional, for faster vector operations)
- Prometheus and Grafana (included in docker-compose.monitoring.yml)

---

### Installation (Local)
* Note: installs all dependencies in a virtual environment 
## Linux/macOS/WSL:
```bash
# Make the setup script executable
chmod +x scripts/setup_uv.sh

# Run the setup script
./scripts/setup_uv.sh

# Activate the virtual environment
source .venv/bin/activate
```

## Windows (PowerShell):
```powershell
# Run the setup script
.\scripts\setup_uv.ps1

# Activate the virtual environment
.venv\Scripts\Activate.ps1
```

# Dockerized Deployment - Docker Desktop Running
## 0. Suggested run in venv from scripts above for your operating system

## 1. **Build and start all basic required services:** 196,293
   ```bash
    docker compose up -d
    # or to rebuild
    docker compose up -d --build api
   ```
 - ✔ Network arxiv_pipeline_default         Started     
 - ✔ Container arxiv_pipeline-neo4j-1       Started     
 - ✔ Container arxiv_pipeline-mongodb-1     Started 
 - ✔ Container arxiv_pipeline-qdrant-1      Started 
 - ✔ Container arxiv_pipeline-web-ui-1      Started   
 - ✔ Container arxiv_pipeline-api-1         Started   
 - ✔ Container arxiv_pipeline-app-1         Started 

## 2. Managing Pipeline Service Containers
   * pipelines do not have to run in order if you have previously run them or starting where you left off
   * recommended to run them in order for processing new papers
   * manual services (Jupyter, Kafka, etc.) are only started when needed with the `--profile manual` flag
  ### a. Run sync_mongodb pipeline to fetch papers from ArXiv API and store in MongoDB:
  ```bash
  # Run the sync-mongodb pipeline container
  docker compose up --build sync-mongodb
  or
   echo $env:MONGO_URI
   $env:MONGO_URI="mongodb://localhost:27017/config"
   python -m src.pipeline.sync_mongodb --config config/default.yaml
  ```

  ### b. Run sync-neo4j pipeline for new pdf metadata inserted from MongoDB:
  ```bash
   docker compose up --build sync-neo4j
   or
   echo $env:MONGO_URI
   $env:MONGO_URI="mongodb://localhost:27017/config"
   echo $env:MONGO_URI
   python -m src.graph.sync_mongo_to_neo4j
  ```

   ### c. Run sync_qdrant pipeline to process downloaded PDFs and store as vector embeddings in Qdrant:
   ```bash
   docker compose up --build sync-qdrant
   or *Need to uncomment docker-compose.yml sync-qdrant service????
   python -m src.pipeline.sync_qdrant
   ```

   ### d. Run download_pdfs pipeline to download PDFs from arxiv.org using metadata stored in MongoDB:
  * pdf download does not have a docker container
   ```bash
   echo $env:MONGO_URI
   $env:MONGO_URI="mongodb://localhost:27017/config"
   echo $env:MONGO_URI
   python -m src.utils.download_pdfs
   ```
## 3. (optional) Managing Monitoring Containers with Prometheus & Grafana
a. **Start the monitoring stack:**
   ```bash
   docker compose -f docker-compose.monitoring.yml up -d
   ```
  or
  **For monitoring containers, use the monitoring compose file:**
  ```bash
  # Start Prometheus
  docker compose -f docker-compose.monitoring.yml start prometheus
  # Start Grafana
  docker compose -f docker-compose.monitoring.yml start grafana
  # View Grafana logs
  docker compose -f docker-compose.monitoring.yml logs grafana
  ```
b. **Access monitoring services:**
   - Prometheus: http://localhost:9090
   - Grafana: http://localhost:3001 (default login: admin/password)

c. **Explore metrics** for:
   - System resources
   - Docker containers
   - MongoDB performance
   - Application-specific metrics

d. **Monitoring Dashboards**
   Pre-configured Grafana dashboards are available in the repository:
   - `config/grafana/dashboards/basic_test_dashboard.json` - Basic connectivity testing dashboard
   - `config/grafana/dashboards/arxiv_data_science_dashboard.json` - Core monitoring for ArXiv pipeline
   - `config/grafana/dashboards/arxiv_advanced_analytics_dashboard.json` - Advanced system correlation metrics
   - `config/grafana/dashboards/arxiv_vector_embedding_dashboard.json` - Vector database performance metrics

   * These dashboards provide visualization for MongoDB operations, system resources, container performance, 
   and vector embedding generation metrics critical for the research paper processing pipeline.

e. **Prometheus Query Documentation**
   Comprehensive documentation for Prometheus queries is available in:
   - `docs/prometheus_basic_queries.md` - Simple queries for troubleshooting
   - `docs/prometheus_queries.md` - General purpose monitoring queries
   - `docs/prometheus_custom_queries.md` - ArXiv pipeline specific metrics
   - `docs/prometheus_working_queries.md` - Verified working queries for dashboards

f. **Monitoring Diagnostics**
   Use the diagnostic script to verify your monitoring setup:
   ```bash
   python scripts/check_prometheus_metrics.py
   ```
   * This script analyzes your Prometheus setup and verifies that critical metrics
   for the ArXiv pipeline are available and functioning correctly.

* Refer to `docs/grafana_dashboard_guide.md` for details on customizing and extending these dashboards.**

## 4. Managing Manual Services

The following services are configured with the `manual` profile in Docker Compose, which means they will only start when explicitly requested:

### a. Jupyter Notebooks for Data Analysis
```bash
# Start Jupyter SciPy notebook server
docker compose --profile manual up jupyter-scipy
# Stop Jupyter SciPy notebook server
docker compose --profile manual down jupyter-scipy
```
* Access at: http://localhost:8888 (check console for token)
* Note: If token access is lost, restart the Jupyter docker container to get a new token

### b. Kafka Messaging System
```bash
# Start Kafka with required Zookeeper service
docker compose --profile manual up zookeeper kafka
# Stop Kafka and Zookeeper services
docker compose --profile manual down zookeeper kafka
```
* Kafka broker accessible at: localhost:9092 (from host) or kafka:9092 (from other containers)

### c. Kafka UI Management Interface
```bash
# Start complete Kafka stack with UI
docker compose --profile manual up zookeeper kafka kafka-ui
# Stop complete Kafka stack
docker compose --profile manual down zookeeper kafka kafka-ui
```
* Access Kafka UI at: http://localhost:8080

## 5. Database Connection Settings
```yaml
mongo:
  connection_string: "mongodb://mongodb:27017/" # or http://localhost:27017
  db_name: "arxiv_papers"
  
neo4j:
  url: "bolt://neo4j:7687"  # or http://localhost:7474
  user: "neo4j"
  password: "password"

qdrant:
  url: "http://localhost:6333" #Access Qdrant UI http://localhost:6333/dashboard
  collection_name: "arxiv_papers"

# Qdrant API Metrics
# The database dashboard displays the following Qdrant metrics:
# - Papers: Number of vector embeddings stored in Qdrant (paper count)
# - Authors: Vector dimensions (typically 768 for research paper embeddings)
# - Categories: Number of collections in Qdrant
  vector_size: 768  # For all-MiniLM-L6-v2 model
```
## 6. Web UI
* To restart Web UI docker service, starts with docker-compose up above:
   ```bash
   docker-compose up -d web-ui
   ```
* Access the web interface at: http://localhost:3000

### Web UI Development Setup

* The web interface uses React with the Neo4j JavaScript driver. If you want to develop the web UI locally:
a. **Navigate to the web-ui directory**:
   ```bash
   cd src/web-ui
   ```

b. **Install dependencies including Neo4j JavaScript driver**:
   ```bash
   npm install
   # Or to install Neo4j driver specifically:
   npm install neo4j-driver@5.13.0
   ```
c. **Start the development server**:
   ```bash
   npm start
   ```
* The web UI connects to Neo4j using environment variables defined in the docker-compose.yml file.

## 7. Data Visualization and Analysis Dashboards

### Paper Analysis Dashboard
The ArXiv Pipeline includes an interactive Paper Analysis Dashboard that provides visual insights into publication trends and patterns. This dashboard is accessible through the MongoDB Reports section of the web interface.

**Key Features:**
- **Time-based Analysis**: View paper publication trends by year, month, or day
- **Multi-dimensional Filtering**: Filter papers by date range, specific year, and research category
- **Dynamic Category Selection**: Choose from the top 50 research categories in your collection
- **Interactive Charts**: Toggle between different time granularities with responsive visualizations
- **Formatted Metrics**: Clear display of total papers with proper numerical formatting

**How to Access:**
1. Navigate to the web UI (http://localhost:3000) when services are running
2. Click on "MongoDB Reports" in the navigation menu
3. Use the filter options to refine your analysis

![Paper Analysis Dashboard](<images/paper_analysis_dashboard.png>)

## 8. Data Validation and Analysis Utilities

The ArXiv Pipeline includes comprehensive data validation and analysis utilities in `src/agents_core/logging_utils.py`. These utilities help ensure data quality, perform temporal analysis, and validate MongoDB collections.

### Paper Schema Validation
```python
from src.agents_core.logging_utils import validate_paper_schema

# Validate a paper document
is_valid, errors = validate_paper_schema(paper_document)
if not is_valid:
    print(f"Paper validation failed with errors: {errors}")
```

### Temporal Analysis
```python
from src.agents_core.logging_utils import count_papers_by_date
from src.storage.mongo import MongoStorage

# Connect to MongoDB
with MongoStorage() as mongo:
    # Count papers by year
    yearly_counts = count_papers_by_date(mongo.papers, date_field="published", group_by="year")
    
    # Count papers by month
    monthly_counts = count_papers_by_date(mongo.papers, date_field="published", group_by="month")
    
    # Count papers by day of week
    weekday_counts = count_papers_by_date(mongo.papers, date_field="published", group_by="weekday")
```

### MongoDB Collection Analysis
```python
from src.agents_core.logging_utils import analyze_mongodb_collection, validate_mongodb_data
from src.storage.mongo import MongoStorage

# Connect to MongoDB
with MongoStorage() as mongo:
    # Analyze collection structure
    analysis = analyze_mongodb_collection(mongo.papers)
    
    # Validate collection data
    validation_results = validate_mongodb_data(mongo.papers, validate_paper_schema)
```

### Data Integrity Checking
```python
from src.agents_core.logging_utils import check_data_integrity
from src.storage.mongo import MongoStorage

# Connect to MongoDB
with MongoStorage() as mongo:
    # Check data integrity with date range
    integrity_results = check_data_integrity(
        mongo.papers, 
        date_range=("2024-01-01T00:00:00Z", "2025-12-31T23:59:59Z")
    )
```

### Formatted Reports
```python
from src.agents_core.logging_utils import generate_date_distribution_report, count_papers_by_date
from src.storage.mongo import MongoStorage

# Connect to MongoDB
with MongoStorage() as mongo:
    # Generate monthly report
    monthly_counts = count_papers_by_date(mongo.papers, group_by="month")
    report = generate_date_distribution_report(monthly_counts, title="Monthly Paper Distribution")
    print(report)
```

These utilities help maintain data quality and provide insights into the ArXiv paper collection. They can be used for monitoring, debugging, and generating reports.

## 9. Managing Individual Docker Containers
* For more fine-grained control over system components, you can start, stop, restart, and inspect specific containers:

### a. Starting Individual Required Containers
* Services: 
```bash
# Start MongoDB
docker compose start mongodb

# Start Neo4j
docker compose start neo4j

# Start Qdrant
docker compose start qdrant

# Start Web UI
docker compose start web-ui

# Start API
docker compose start api

# Start APP
docker compose start app
```

### b. Restarting Individual Containers

```bash
# Restart MongoDB

docker compose restart mongodb

# Restart Neo4j

docker compose restart neo4j

# Restart Qdrant

docker compose restart qdrant

# Restart Web UI

docker compose restart web-ui
```

### c. Viewing Container Logs

```bash

# View MongoDB logs

docker compose logs mongodb

# View Neo4j logs

docker compose logs neo4j

# View Qdrant logs

docker compose logs qdrant

# View Web UI logs

docker compose logs web-ui

# Follow logs (real-time updates)

docker compose logs --follow mongodb

```

### d. Inspecting Container Status

```bash
# Check status of all containers

docker compose ps

# Detailed information about a specific container

docker inspect arxiv_pipeline-mongodb-1
```

## 6. Optional: GPU-Accelerated Qdrant Setup on Remote Windows Machine

* For enhanced vector search performance, you can set up Qdrant with GPU acceleration on a separate Windows machine within the same network. This configuration is beneficial for:
- Processing large volumes of papers with faster embedding searches
- Leveraging dedicated GPU resources for vector operations
- Scaling the vector database independently from other components

### Quick Overview

a. **Hardware Requirements**:

   - Windows 11 with WSL2 enabled
   - NVIDIA GPU with CUDA 12.x support (8GB VRAM minimum)
   - 16GB RAM (32GB recommended)
   - IP address on your local network


b. **Setup Approach**:

   - Install WSL2 with Ubuntu

   - Configure CUDA in WSL2

   - Build Qdrant from source with GPU support

   - Configure for optimal performance with research paper embeddings


c. **Integration with ArXiv Pipeline**:

   - After setup, update the Qdrant connection settings in your config/default.yaml

   - Run the pipeline as usual, with vector operations now GPU-accelerated

### Detailed Instructions

* Complete step-by-step instructions are available in the `qdrant_setup` directory:

```bash

# View the detailed setup guide

cat qdrant_setup/README.md

```

#### The guide includes:

- Full installation procedures

- Configuration optimized for 768-dimensional embeddings (typical for research papers)

- Testing and benchmarking tools

- Maintenance and backup procedures

- Security recommendations

## Updating Configuration

* After setting up GPU-accelerated Qdrant, update your configuration:

```yaml

# In config/default.yaml

qdrant:

  host: "192.168.1.x"  # Replace with your Qdrant server's IP

  port: 6333

  collection_name: "arxiv_papers"

```

* **New Feature:** The sync_qdrant pipeline now includes **MongoDB tracking** to prevent duplicate processing of PDFs. Each processed PDF is recorded in the `vector_processed_pdfs` collection with metadata including file hash, processing date, and chunk count.



---

### Configuration

![Image](https://github.com/user-attachments/assets/7d68b38e-b4a1-49d9-acf4-17b74fb05e22)

The application is configured using YAML files in the `config/` directory. The default configuration is in `config/default.yaml`.

Key configuration options:

## Recent Feature Additions

### 1. MongoDB Tracking for Qdrant Vector Processing

* The sync_qdrant pipeline now includes a tracking system to prevent duplicate processing and provide synchronization with Qdrant:

```yaml

# In config/default.yaml

qdrant:

  # ... other settings ...

  tracking:

    enabled: true # Whether to track processed PDFs

    collection_name: "vector_processed_pdfs" # MongoDB collection to store tracking information

    sync_with_qdrant: true # Whether to sync tracking with actual Qdrant contents

```

### This system:

- Tracks each processed PDF in a MongoDB collection

- Prevents duplicate processing of the same document

- Stores metadata including file hash, processing date, and chunk count

- Maintains consistency between MongoDB tracking and Qdrant vector storage

### 2. GPU Acceleration for Vector Operations

The pipeline now supports GPU acceleration for both:

#### A. Qdrant Vector Database

```yaml
# In config/default.yaml
qdrant:
  # ... other settings ...
  gpu_enabled: true # Enable GPU for vector operations
  gpu_device: 1 # GPU device index (0 for first GPU, 1 for second, etc.)
```

#### B. Standalone Qdrant with GPU
For better performance with large vector collections, you can run Qdrant as a standalone application with GPU support as documented in the "Qdrant Deployment Options" section.

---

## Ollama Integration (Optional)

The `sync_qdrant` pipeline uses [Ollama](https://ollama.ai/) app for analyzing images extracted from PDFs if available:
- **What Ollama does**: Enhances the vector database by adding AI-generated descriptions of diagrams and figures in papers
- **Installation Options**:
  - **Desktop App**: Download and install Ollama from [ollama.ai](https://ollama.ai/)
  - **Docker Container**: Run Ollama in a Docker container for easier integration with the ArXiv pipeline (see `docs/ollama_docker.md` for detailed instructions)
- **Required model**: Run `ollama pull llama3` to download the required model
- **Without Ollama**: The pipeline still functions normally without Ollama, but image descriptions will be placeholders

## ArXiv Pipeline Configuration Settings

The system is configured through `config/default.yaml`. Key configuration sections included

### Important Note About PDF Paths in Docker

When running the `sync_qdrant` service in Docker, the PDF directory path specified in `config/default.yaml` is overridden by the volume mapping in `docker-compose.yml`:

```yaml

# In docker-compose.yml

volumes:
  - E:/AI Research:/app/data/pdfs  # Maps Windows path to container path
```

This means:
- Your PDFs should be stored in `E:/AI Research` on your Windows machine
- Inside the Docker container, they will be accessible at `/app/data/pdfs`
- The script automatically detects when running in Docker and adjusts paths accordingly

If you change your PDF storage location, make sure to update both:
1. The `pdf_storage.directory` in `config/default.yaml` (for local runs)
2. The volume mapping in `docker-compose.yml` (for Docker runs)
   ## sync_mongodb pipeline

   - arxiv.categories: Research categories to fetch papers from api into mongodb

   - arxiv.max_results: Number of papers to fetch per API call

   - arxiv.rate_limit_seconds: Number of seconds to wait between API calls

   - arxiv.max_iterations: Number of API calls per category

   - arxiv.start_date: Only process papers published after this date

   - arxiv.end_date: Only process papers published before this date



   ## sync_neo4j pipeline

   - arxiv.process_categories: Categories to prioritize for vector storage into qdrant

   - arxiv.max_papers: Maximum number of papers to process

   - arxiv.max_papers_per_category: Maximum number of papers to insert per category

   - arxiv.sort_by: Sort papers by this field

   - arxiv.sort_order: Sort papers in this order



   ## sync_qdrant pipeline

   - arxiv.max_papers: Maximum number of papers to process

   - arxiv.max_papers_per_category: Maximum number of papers to insert per category

   - arxiv.sort_by: Sort papers by this field

   - arxiv.sort_order: Sort papers in this order



   ## download_pdfs pipeline

   - arxiv.max_papers: Maximum number of papers to process

   - arxiv.max_papers_per_category: Maximum number of papers to download per category

   - arxiv.sort_by: Sort papers by this field

   - arxiv.sort_order: Sort papers in this order


Config changes take effect when services are restarted. See `docs/system_design.md` for detailed information about configuration impact on system behavior.

## Qdrant Deployment Options

This pipeline supports two options for running Qdrant (vector database):

### Option 1: Running Qdrant in Docker (Default)

In the `docker-compose.yml` file, we provide a pre-configured Qdrant container:

```yaml
qdrant:
  image: qdrant/qdrant:latest
  ports:
    - "6333:6333"
    - "6334:6334"
  volumes:
    - qdrant_data:/qdrant/storage
  restart: unless-stopped
```

### Option 2: Running Qdrant Locally with GPU Support

For better performance with large vector collections, you can run Qdrant as a standalone application with GPU acceleration:

1. **Download Qdrant** from [GitHub Releases](https://github.com/qdrant/qdrant/releases)
2. **Create a config file** at `config/qdrant_config.yaml` with GPU settings:

```yaml
storage:
  # Path to the directory where collections will be stored
  storage_path: ./storage
  # Vector data configuration with GPU support
  vector_data:
    # Enable CUDA support
    enable_cuda: true
    # GPU device index (0 for first GPU, 1 for second, etc.)
    cuda_device: 0
```

3. **Run Qdrant with the config**:

```
qdrant.exe --config-path config/qdrant_config.yaml
```

4. **Update the docker-compose.yml file** to comment out the Qdrant service but keep other services:
```yaml
# Comment out the Qdrant service
#qdrant:
#  image: qdrant/qdrant:latest
#  ...
# Update service connections to use host.docker.internal
app:
  environment:
    - QDRANT_URL=http://host.docker.internal:6333
```

## GPU Support for Embeddings Generation

* The pipeline can use GPU acceleration for generating embeddings in the `sync_qdrant.py` script:

1. **Install PyTorch with CUDA support**:
```bash

pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

```

* Choose the appropriate CUDA version (cu118, cu121, etc.) based on your system. Check with `nvidia-smi`.

2. **Enable GPU in configuration**:
```yaml
# In config/default.yaml
qdrant:
  gpu_enabled: true  # Enable GPU for vector operations
  gpu_device: 0      # GPU device index (0 for first GPU)
```

3. **Verify GPU detection** by checking script output when running:
```

Using GPU for embeddings: cuda:0

```
## Database Installation & Connection Settings
### MongoDB Installation
#### Option 1: With Docker (recommended)
The Docker setup includes MongoDB, so no additional installation is needed if using Docker Compose.
#### Option 2: Standalone MongoDB Installation
1. **Download MongoDB Community Server**: [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)
2. **Install Python driver**:
   ```bash
   pip install pymongo>=4.3.0
   ```

3. **Test your connection**:
   ```python
   from pymongo import MongoClient
   client = MongoClient('mongodb://localhost:27017/')
   db = client['arxiv_papers']
   print(f"Connected to MongoDB: {client.server_info()['version']}")
   ```

### Neo4j Installation

#### Option 1: With Docker (recommended)
* The Docker setup includes Neo4j, so no additional installation is needed if using Docker Compose.
* Neo4j Desktop is recommended for local development and data exploration.

#### Option 2: Standalone Neo4j Installation
1. **Download Neo4j Desktop**: [https://neo4j.com/download/](https://neo4j.com/download/)
2. **Create a new database** with password 'password' to match configuration
3. **Install Python driver**:

   ```bash
   pip install neo4j>=5.5.0
   ```

4. **Test your connection**:

   ```python
   from neo4j import GraphDatabase
   uri = "bolt://localhost:7687"
   driver = GraphDatabase.driver(uri, auth=("neo4j", "password"))
   with driver.session() as session:
       result = session.run("MATCH (n) RETURN count(n) AS count")
       print(result.single()["count"])
   driver.close()
   ```

---

## Notes

- **Python Versions**: 
  - Docker containers use `python:3.11-slim`
  - Local development 'requires' Python ≥3.11 as specified in pyproject.toml
  - All dependencies are managed through pyproject.toml for consistent environments

- **Data Persistence**:
  - All persistent data (MongoDB, Neo4j, Qdrant) is stored in Docker volumes or local directories
  - PDF files are stored in the configured local directory

- **Development Approach**:
  - Either use the Python virtual environment with `python -m` commands
  - Or use Docker Compose for containerized execution
  - Both methods use the same configuration and produce consistent results
  - Deveoper commonly uses both methods running in python env, docker compose and standalone databases

---

## Troubleshooting

- If you see `ModuleNotFoundError: No module named 'pymongo'`, ensure you have activated your virtual environment and installed dependencies.
- For Docker issues, ensure Docker Desktop is running and you have sufficient permissions.

---

## External Tools for Data Exploration

* The following tools are recommended for exploring the data outside the pipeline:

### MongoDB

- **MongoDB Compass** - A GUI for MongoDB that allows you to explore databases, collections, and documents
- Download: [https://www.mongodb.com/products/compass](https://www.mongodb.com/products/compass)
- Connection string: `mongodb://localhost:27017/onfig` (when connecting to the Docker container)

### Neo4j
- **Neo4j Desktop** - A complete development environment for Neo4j projects
- Download: [https://neo4j.com/download/](https://neo4j.com/download/)
- Or use the Neo4j Browser at: http://localhost:7474/ (default credentials: neo4j/password)

### Qdrant
- **Qdrant Web UI** - A built-in web interface for exploring vector collections
- Access at: http://localhost:6333/dashboard when Qdrant is running
- Also consider **Qdrant Cloud Console** for more advanced features if you're using Qdrant Cloud
- Check Jupyter notebooks for more advanced features
These tools provide graphical interfaces to explore, query, and visualize the data stored in each component of the pipeline.
---
## 📊 Optional Future Enhancements
The following features are 'planned' for future development to enhance the research pipeline:
### Data Analysis and Visualization
- **Topic Modeling**: Implement BERTopic or LDA for automatic discovery of research themes
- **Time-Series Analysis**: Track the evolution of research topics over time

### Research Enhancement Tools
- **PDF Section Parsing**: Intelligently extract structured sections from research papers (abstract, methods, results, etc.)
- **Citation Parsing**: Extract and normalize citations from paper references
- **Mathematical Model Extraction**: Identify and extract mathematical formulas and models from papers
- **Citation Graph Analysis**: Build a graph of paper citations to identify seminal works
- **Researcher Networks**: Map collaboration networks among authors
- **Multi-Modal Analysis**: Extract and analyze figures and tables from papers
- **Fine-tuning Pipelines**: Fine-tune pipelines for specific use cases
- **Research Agents**: Add specific research agents for specific use cases

### Infrastructure Improvements
- **LangChain-based Research Assistant**: Natural language interface to query the database
- **Hybrid Search**: Combine keyword and semantic search for better results
- **Export Tools**: Add BibTeX and PDF collection exports
- **Web Admin Interface**: Add web admin interface for configuration and running pipelines

## To-Do List
- [ ] **Short-term Tasks**
  - [ ] Optimize PDF download with parallel processing
  - [ ] Add citation extraction from PDF full text
  - [ ] Implement paper similarity metrics
  - [ ] Create basic analytics dashboard
  - [ ] Develop basic PDF section parser to extract abstracts and conclusions
  - [ ] Add web admin interface for configuration and running pipelines

- [ ] **Medium-term Tasks**
  - [ ] Fine-tuning pipelines for specific use cases
  - [ ] Add research agent for specific use cases
  - [ ] Extend Neo4j schema to include citations between papers
  - [ ] Add full-text search capabilities
  - [ ] Implement comprehensive citation parsing system
  - [x] Create example Jupyter notebooks for research workflows
  - [ ] Develop mathematical formula extraction and indexing
  - [ ] Implement automated paper summarization
  - [ ] Set up scheduled runs for continuous updates

- [ ] **Long-term Tasks**
  - [ ] Build a recommendation system for related papers
  - [ ] Develop a natural language query interface
  - [ ] Create a researcher profile system
  - [ ] Add support for other research paper repositories (e.g., PubMed, IEEE)

- [ ] **Infrastructure Tasks**
  - [x] Add Prometheus/Grafana for monitoring
  - [ ] Implement automated testing
  - [ ] Set up CI/CD pipeline for continuous deployment
  - [ ] Optimize vector storage for large-scale collections
---

## ArXiv API Address to fetch papers metadata

http://export.arxiv.org/api/query

List used is in config/defaults.yaml for reference, more categories available. 

---

- cs.AI - Artificial Intelligence
- cs.CL - Computation and Language
- cs.CV - Computer Vision and Pattern Recognition
- cs.DS - Data Structures and Algorithms
- cs.GT - Computer Science and Game Theory
- cs.LG - Machine Learning
- cs.LO - Logic in Computer Science
- cs.MA - Multiagent Systems
- cs.NA - Numerical Analysis
- cs.NE - Neural and Evolutionary Computing
- math.PR - Probability
- q-bio.NC - Neurons and Cognition
- stat - Statistics
- stat.ML - Machine Learning
- stat.TH - Statistics Theory
- physics.data-an - Data Analysis, Statistics and Probability
---
For more details about project and status, see the `docs/` directory.

