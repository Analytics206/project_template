# WhisperWell Development Environment Setup

## System Requirements

- **Operating System**: Windows 11 (64-bit)
- **Memory**: 16GB RAM (32GB recommended for AI/ML development)
- **Storage**: 50GB free space (SSD recommended)
- **GPU**: NVIDIA GPU with CUDA support (recommended for ML tasks)

## Prerequisites

### 1. Windows Subsystem for Linux (WSL 2)

```powershell
# Enable WSL
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# Enable Virtual Machine Platform
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# Set WSL 2 as default
wsl --set-default-version 2

# Install Ubuntu 24.04 LTS from Microsoft Store
# After installation, launch Ubuntu and complete setup
```

### 2. Docker Desktop for Windows

1. Download and install [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/)
2. Enable WSL 2 backend in Docker Desktop settings
3. Allocate at least 8GB RAM to Docker in Settings > Resources

### 3. Node.js & NPM

```powershell
# Install nvm-windows
choco install nvm

# Install Node.js LTS
nvm install 18.17.0
nvm use 18.17.0
```

### 4. Python 3.11

```powershell
# Install Python 3.11
choco install python --version=3.11.0

# Verify installation
python --version
pip --version

# Install essential Python packages
pip install --upgrade pip setuptools wheel
pip install virtualenv virtualenvwrapper-win
```

### 5. Git

```powershell
# Install Git
choco install git

# Configure Git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.autocrlf true
```

## VS Code Setup

### 1. Install VS Code

Download and install [Visual Studio Code](https://code.visualstudio.com/)

### 2. Recommended Extensions

```json
{
    "recommendations": [
        "ms-vscode-remote.remote-wsl",
        "ms-vscode-remote.remote-containers",
        "ms-vscode-remote.remote-ssh",
        "ms-azuretools.vscode-docker",
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "ms-python.python",
        "ms-python.vscode-pylance",
        "ms-vscode.vscode-typescript-next",
        "graphql.vscode-graphql",
        "apollographql.vscode-apollo",
        "mongodb.mongodb-vscode",
        "ms-kubernetes-tools.vscode-kubernetes-tools",
        "redhat.vscode-yaml",
        "editorconfig.editorconfig",
        "github.copilot",
        "github.vscode-github-actions",
        "eamodio.gitlens",
        "prisma.prisma"
    ]
}
```

### 3. VS Code Settings

Create/update `.vscode/settings.json` in your project:

```json
{
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": "explicit",
        "source.organizeImports": true
    },
    "eslint.validate": ["javascript", "javascriptreact", "typescript", "typescriptreact"],
    "python.analysis.typeCheckingMode": "basic",
    "python.linting.enabled": true,
    "python.linting.pylintEnabled": true,
    "python.formatting.provider": "black",
    "python.languageServer": "Pylance",
    "python.analysis.autoImportCompletions": true,
    "typescript.tsdk": "node_modules/typescript/lib",
    "typescript.preferences.importModuleSpecifier": "relative",
    "files.autoSave": "onFocusChange",
    "files.exclude": {
        "**/.git": true,
        "**/.DS_Store": true,
        "**/node_modules": true,
        "**/__pycache__": true,
        "**/.pytest_cache": true
    },
    "terminal.integrated.defaultProfile.windows": "Git Bash"
}
```

## Project Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-org/whisperwell.git
cd whisperwell
```

# 1. Check what changed
git status

# 2. Stage changes
git add .

# 3. Commit with a message
git commit -m "Your commit message"

# 4. Push to GitHub
git push

### 2. Install Dependencies

#### Frontend

```bash
cd apps/web
npm install
```

#### Backend

```bash
cd apps/server
python -m venv venv
.\venv\Scripts\activate  # On Windows
pip install -r requirements-dev.txt
```

### 3. Configure Environment Variables

Create `.env` files in each service directory:

```env
# .env.development
NODE_ENV=development
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_WS_URL=ws://localhost:3001
```

### 4. Start Development Servers

#### Frontend

```bash
cd apps/web
npm run dev
```

#### Backend

```bash
cd apps/server
uvicorn main:app --reload
```

## Development Workflow

### 1. Git Workflow

```bash
# Create a new branch
git checkout -b feature/your-feature-name

# Stage changes
git add .

# Commit changes
git commit -m "feat: add new feature"

# Push to remote
git push -u origin feature/your-feature-name

# Create a pull request from GitHub/GitLab UI
```

### 2. Code Quality

- ESLint for TypeScript/JavaScript
- Prettier for code formatting
- Pylint for Python
- Pre-commit hooks for automated checks

### 3. Testing

```bash
# Run frontend tests
npm test

# Run backend tests
pytest

# Run end-to-end tests
npm run test:e2e
```

## Database Setup

### 1. MongoDB

```bash
# Using Docker
docker run -d --name mongodb -p 27017:27017 -v mongodb_data:/data/db mongo:6.0
```

### 2. Qdrant (Vector Database)

```bash
# Using Docker with GPU support
docker run -d --name qdrant -p 6333:6333 -p 6334:6334 \
  -v $(pwd)/qdrant_storage:/qdrant/storage \
  qdrant/qdrant
```

### 3. Neo4j

```bash
# Using Docker
docker run -d --name neo4j \
  -p 7474:7474 -p 7687:7687 \
  -v neo4j_data:/data \
  -v neo4j_logs:/logs \
  -v neo4j_import:/var/lib/neo4j/import \
  --env NEO4J_AUTH=neo4j/yourpassword \
  neo4j:5.0
```

## AI/ML Development

### 1. PyTorch with CUDA

```bash
# Install PyTorch with CUDA support
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### 2. Hugging Face Transformers

```bash
pip install transformers datasets evaluate
```

## Troubleshooting

### Common Issues

1. **WSL 2 Installation Issues**
   - Ensure virtualization is enabled in BIOS
   - Run `wsl --update` to ensure latest version

2. **Docker Desktop Not Starting**
   - Run Docker Desktop as administrator
   - Reset to factory defaults if needed

3. **Python Environment Issues**
   - Always use virtual environments
   - Ensure Python path is correctly set in VS Code

4. **Node.js Version Conflicts**
   - Use nvm-windows to manage Node.js versions
   - Run `nvm use` in project directory if .nvmrc exists

## Resources

- [VS Code Documentation](https://code.visualstudio.com/docs)
- [Docker Documentation](https://docs.docker.com/)
- [Node.js Documentation](https://nodejs.org/en/docs/)
- [Python Documentation](https://docs.python.org/3/)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [Neo4j Documentation](https://neo4j.com/docs/)

## Support

For any issues, please:
1. Check the troubleshooting section
2. Search existing issues
3. Open a new issue with detailed information

---
*Last updated: 2025-05-22*