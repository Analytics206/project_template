# NEW PROJECT NAME
---
## Version 0.2.0 (May 3, 2025)

### Major Features

#### PDF Processing and Vector Storage
- **MongoDB Tracking System** - Added tracking of processed PDFs in `vector_processed_pdfs` collection to prevent duplicate processing
- **PDF Processing Tracking** - Each processed PDF is tracked with file hash, chunk count, and processing date
- **Category-Based Processing** - Implemented selective vector processing based on configured research categories
- **Papers per Category Limit** - Added configurable limit for papers to process per category

#### GPU Acceleration
- **GPU Support for Vector Operations** - Added GPU acceleration for both Qdrant vector database and embedding generation
- **Multi-GPU Support** - Implemented configurable GPU device selection for optimal performance
- **Automatic Device Detection** - Added graceful fallback to CPU when GPU is unavailable or not properly configured

#### Deployment Improvements
- **Hybrid Deployment Architecture** - Added support for running Qdrant locally with GPU while other services run in Docker
- **Host.Docker.Internal Integration** - Enhanced Docker services to communicate with local Qdrant instance
- **Standalone Qdrant Configuration** - Added documentation for running Qdrant with GPU acceleration
- **Docker Volume Path Handling** - Improved Windows path compatibility for mounted volumes

#### Error Handling
- **Ollama Integration Improvements** - Made Ollama optional with graceful fallback when not available
- **Better Error Recovery** - Added robust error handling for PDF processing failures

### Configuration Enhancements
- **Centralized PDF Directory Config** - Moved PDF directory configuration to central config file
- **Dynamic MongoDB Connection** - Improved connection handling to automatically adjust for local vs Docker environments
- **Ollama Configuration** - Added controls for enabling/disabling Ollama image analysis

### Documentation
- **Deployment Options** - Added documentation for both Docker and standalone deployment options
- **GPU Configuration Guide** - Documented GPU setup and acceleration options
- **Database Installation Guides** - Added detailed instructions for MongoDB, Neo4j, and Qdrant installation
- **Development Notes** - Added developer notes document for tracking ongoing work
- **Release Notes** - Added this release notes document

### Dependencies and Libraries
- **PyTorch with CUDA** - Updated PyTorch requirements to include CUDA support
- **Neo4j JavaScript Driver** - Added documentation for the JS driver required for the web UI

---

## Version 0.1.0 (April 26, 2025)

### Major Features

#### Data Ingestion and Storage
- **ArXiv API Integration** - Implemented paper ingestion from ArXiv Atom XML API
- **MongoDB Storage** - Created document storage for paper metadata with appropriate indexing
- **Neo4j Graph Database** - Established graph representation for papers, authors, and categories
- **PDF Downloading** - Added functionality to download and organize research papers in PDF format
- **Vector Embedding** - Implemented basic text vectorization using Hugging Face models
- **Qdrant Integration** - Set up vector similarity search with Qdrant database

#### Docker Containerization
- **Multi-Container Setup** - Built initial Docker Compose configuration for all services
- **Volume Persistence** - Implemented persistent storage for MongoDB and Neo4j data
- **Network Configuration** - Established internal container communication and port mapping
- **Service Orchestration** - Created coordinated startup/shutdown of all system components

#### Web Interface
- **Neo4j Visualization** - Created basic web interface for exploring the knowledge graph
- **Browsing Interface** - Implemented paper browsing and navigation features
- **Web UI Container** - Dockerized the web interface with appropriate connections to backend services

### Configuration Enhancements
- **YAML Configuration** - Created initial configuration file structure
- **Environment Variables** - Implemented environment variable support for container configuration
- **API Rate Limiting** - Added configurable rate limiting for ArXiv API access

### Documentation
- **Setup Instructions** - Created installation and setup documentation
- **README** - Established initial project documentation with overview and features
- **Configuration Guide** - Documented configuration options and their effects

### Dependencies and Libraries
- **MongoDB Python Driver** - Integrated PyMongo for database access
- **Neo4j Python Driver** - Added Neo4j connectivity for graph operations
- **Hugging Face Transformers** - Integrated for text embedding generation
- **Docker and Docker Compose** - Established containerization foundation
